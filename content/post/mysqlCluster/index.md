---
title: 搭建MySQLCluster集群环境
slug: mysqlCluster
date: 2023-11-07 10:30:00+0800
image: mysql.png
categories:
    - techStudy
tags:
    - mysql
    - Ubuntu
    - docker
#weight: 1       # You can add weight to some posts to override the default sorting (date descending)
comments: true

#license: flase
#math: true
#toc: true
#style: 
#keywords:
#readingTime:
---

## 环境

- windows11上的ubuntu22.04的wsl2
- ubuntu20.04
- mysql-cluster_8.0.35-1ubuntu20.04_amd64 


## 配置环境

- sql0 192.168.50.100 管理节点
- sql1 192.168.50.128 数据节点[11] sql节点
- sql2 192.168.50.129 数据节点[12] sql节点

### docker容器配置

- windows11上的ubuntu22.04的wsl2

1. 下载镜像

```shell
docker pull ubuntu:20.04
```

2. 创建网络

```shell
docker network create --driver bridge --subnet 192.168.50.0/24 --gateway 192.168.50.1 mysqlBridge
```

3. 创建容器

```shell
# sql0
docker run -di --name sql0 -v /home/lihan/sqlStudy:/home/shareFiles --net mysqlBridge --ip 192.168.50.100 ubuntu:20.04

```

4. 下载`mysql-cluster-community-server`安装包

- [/mysql-cluster_8.0.35-1ubuntu20.04_amd64.deb-bundle.tar](https://dev.mysql.com/get/Downloads/MySQL-Cluster-8.0/mysql-cluster_8.0.35-1ubuntu20.04_amd64.deb-bundle.tar)

将下载好的安装包放在宿主机挂载的目录下

5. 安装`mysql-cluster-community-server`

- sql0

```shell
# 进入容器
docker exec -it sql0 /bin/bash

# 创建mysql用户
adduser mysql 
# 密码123456
usermod -aG sudo mysql

# 解压文件到install目录
mkdir install
tar -xvf /home/shareFiles/mysql-cluster_8.0.35-1ubuntu20.04_amd64.deb-bundle.tar  -C install/
cd install
```

- 缺少依赖`libssl1.1`
- [libssl1.1_1.1.1-1ubuntu2.1~18.04.23_amd64](http://security.ubuntu.com/ubuntu/pool/main/o/openssl/libssl1.1_1.1.1-1ubuntu2.1~18.04.23_amd64.deb)

```shell
# 下载依赖包`libssl1.1`后，复制到宿主机挂载目录下

dpkg -i /home/shareFiles/libssl1.1_1.1.1-1ubuntu2.1~18.04.23_amd64.deb

# 更新

apt update
apt upgrade

```

6. 制作镜像并创建`sql1`和`sql2`容器

- sql0

```shell
# 退出容器
exit

docker stop sql0

docker commit sql0 lihansql:1.0

docker start sql0
# (可选)镜像保存为文件
docker save -o lihansql_1.0.tar lihansql:1.0

# 根据镜像创建容器sql1 sql2

# sql1
docker run -di --name sql1 -v /home/lihan/sqlStudy:/home/shareFiles --net mysqlBridge --ip 192.168.50.128 lihansql:1.0
# sql2
docker run -di --name sql2 -v /home/lihan/sqlStudy:/home/shareFiles --net mysqlBridge --ip 192.168.50.129 lihansql:1.0

```

### 配置集群管理器(Cluster Manager服务器)

- sql0 192.168.50.100

1. 安装`ndb_mgmd`

```shell
dpkg -i install/mysql-cluster-community-management-server_8.0.35-1ubuntu20.04_amd64.deb
```

2. 配置`ndb_mgmd`

```shell
# 创建配置文件
mkdir /var/lib/mysql-cluster
vim /var/lib/mysql-cluster/config.ini
```

- config.ini

```ini
[ndbd default]
# Options affecting ndbd processes on all data nodes:
NoOfReplicas=2  
# Number of replicas

[ndb_mgmd]
# Management process options:
hostname=192.168.50.100  
# Hostname of the manager
datadir=/var/lib/mysql-cluster  
# Directory for the log files

[ndbd]
hostname=192.168.50.128 
# Hostname/IP of the first data node
NodeId=11            
# Node ID for this data node
datadir=/usr/local/mysql/data   
# Remote directory for the data files

[ndbd]
hostname=192.168.50.129 
# Hostname/IP of the second data node
NodeId=12            
# Node ID for this data node
datadir=/usr/local/mysql/data   
# Remote directory for the data files

[mysqld]
# SQL node options:
hostname=192.168.50.128 
# MySQL server/client i manager

[mysqld]
# SQL node options:
hostname=192.168.50.129 
# MySQL server/client i manager
```

3. 启动`ndb_mgmd`

```shell
ndb_mgmd -f /var/lib/mysql-cluster/config.ini
```

显示以下信息

```shell
MySQL Cluster Management Server mysql-8.0.35 ndb-8.0.35
2023-11-07 08:24:08 [MgmtSrvr] INFO     -- The default config directory '/usr/mysql-cluster' does not exist. Trying to create it...
2023-11-07 08:24:08 [MgmtSrvr] INFO     -- Sucessfully created config directory
```

4. 配置`ndb_mgmd`开机启动

```shell
# 杀死进程
pkill -f ndb_mgmd

# 创建启动脚本
vim /etc/systemd/system/ndb_mgmd.service

# 编辑ndb_mgmd.service

[Unit]
Description=MySQL NDB Cluster Management Server
After=network.target auditd.service

[Service]
Type=forking
ExecStart=/usr/sbin/ndb_mgmd -f /var/lib/mysql-cluster/config.ini
ExecReload=/bin/kill -HUP $MAINPID
KillMode=process
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

5. 管理`ndb_mgmd`

```shell
# 采用daemon-reload重新加载配置
systemctl daemon-reload

# 启动ndb_mgmd开机运行
systemctl enable ndb_mgmd

# 启动ndb_mgmd
systemctl start ndb_mgmd

# 验证ndb_mgmd是否正在执行
systemctl status ndb_mgmd

# 应该输出类似信息
ndb_mgmd.service - MySQL NDB Cluster Management Server
    Loaded: loaded (/etc/systemd/system/ndb_mgmd.service, enabled)
    Active: active (running)

# 设置允许其他MySQL Cluster节点接入，如无`ufw`等防火墙，可以跳过这一步
ufw allow from 192.168.50.100
ufw allow from 192.168.50.128
ufw allow from 192.168.50.129

```

### 配置数据节点(Data Nodes)

- sql1 192.168.50.128

1. 安装`ndbd`

```shell
# 安装依赖
sudo apt-get –f install 
sudo apt install libclass-methodmaker-perl

# 安装ndbd
dpkg -i install/mysql-cluster-community-data-node_8.0.35-1ubuntu20.04_amd64.deb
```

2. 创建并配置 配置文件

```shell
# 创建配置文件
vim /etc/my.cnf

# 编辑my.cnf

[mysql_cluster]
# Options for NDB Cluster processes:
ndb-connectstring=192.168.50.100  
# location of cluster manager

# 创建数据目录
mkdir -p /usr/local/mysql/data
```

3. 启动`ndbd`

```shell
# 启动
ndbd

# 输出类似信息
2023-11-08 00:47:06 [ndbd] INFO     -- Angel connected to '192.168.50.100:1186'
2023-11-08 00:47:07 [ndbd] INFO     -- Angel allocated nodeid: 11

# 如果出现连接问题，请打开防火墙
ufw allow from 192.168.50.100
ufw allow from 192.168.50.128
ufw allow from 192.168.50.129
```

4. 配置`ndbd`开机启动

```shell
# 杀死进程
pkill -f ndbd

# 创建启动脚本
vim /etc/systemd/system/ndbd.service

# 编辑ndbd.service

[Unit]
Description=MySQL NDB Data Node Daemon
After=network.target auditd.service

[Service]
Type=forking
ExecStart=/usr/sbin/ndbd
ExecReload=/bin/kill -HUP $MAINPID
KillMode=process
Restart=on-failure

[Install]
WantedBy=multi-user.target

```

5. 管理`ndbd`

```shell
# 采用daemon-reload重新加载配置
systemctl daemon-reload

# 启动ndb_mgmd开机运行
systemctl enable ndbd

# 启动ndb_mgmd
systemctl start ndbd

# 验证ndb_mgmd是否正在执行
systemctl status ndbd

# 应该输出类似信息
ndbd.service - MySQL NDB Data Node Daemon
    Loaded: loaded (/etc/systemd/system/ndbd.service, enabled)
    Active: active (running)
```

### 配置SQL节点(配置并运行MySQL Server 和 Client)

- sql1
- 标准的MySQL server不支持 MySQL Cluster 引擎 NDB. 这意味着我们需要安装含有定制的SQL服务器 MySQL Cluster软件.

1. 安装依赖

```shell
# 安装联网依赖
apt update
apt install libaio1 libmecab2

# 解压安装install目录下的依赖包
dpkg -i mysql-common_8.0.35-1ubuntu16.04_amd64.deb

dpkg -i install/mysql-cluster-community-client-plugins_8.0.35-1ubuntu20.04_amd64.deb 

# 安装提示缺少依赖`libgssapi-krb5-2` `libkrb5-3` `libsasl2-2`
apt --fix-broken install

# 继续安装
dpkg -i install/mysql-cluster-community-client-plugins_8.0.35-1ubuntu20.04_amd64.deb 

dpkg -i install/mysql-cluster-community-client-core_8.0.35-1ubuntu20.04_amd64.deb

dpkg -i install/mysql-cluster-community-client_8.0.35-1ubuntu20.04_amd64.deb

dpkg -i install/mysql-client_8.0.35-1ubuntu20.04_amd64.deb

dpkg -i install/mysql-cluster-community-server-core_8.0.35-1ubuntu20.04_amd64.deb
# 安装提示缺少依赖`libnuma1`
apt --fix-broken install

# 继续安装
dpkg -i install/mysql-cluster-community-server-core_8.0.35-1ubuntu20.04_amd64.deb

dpkg -i install/mysql-cluster-community-server_8.0.35-1ubuntu20.04_amd64.deb
# 安装提示缺少依赖`libnuma1`
apt --fix-broken install

# 继续安装
dpkg -i install/mysql-cluster-community-server_8.0.35-1ubuntu20.04_amd64.deb

#提示设置root密码
cuc
# 报错`systemd`
apt install systemd
# 重新安装
dpkg -i install/mysql-cluster-community-server_8.0.35-1ubuntu20.04_amd64.deb

dpkg -i install/mysql-server_8.0.35-1ubuntu20.04_amd64.deb
```

2. 配置`MySQL server`

- 如果出现`dialog`报错
`apt install dialog`

```shell
# MySQL Server 配置文件默认为 /etc/mysql/my.cnf
vim /etc/mysql/my.cnf

# 编辑my.cnf
[mysqld]
# Options for mysqld process:
ndbcluster                      
# run NDB storage engine

[mysql_cluster]
# Options for NDB Cluster processes:
ndb-connectstring=192.168.50.100  
# location of management server

# 重启
systemctl restart mysql

# 开机启动
systemctl enable mysql
```
 
3. 制作镜像并创建`sql2`容器

```shell















