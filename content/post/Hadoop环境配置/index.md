---
title: 搭建Hadoop环境
slug: Hadoop
date: 2023-12-19 10:30:00+0800
image: Hadoop.png
categories:
    - cucStudy
tags:
    - Hadoop
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
- ubuntu22.04
- hadoop-3.1.2
- hbase-1.2.11
- jdk-8u201-linux-x64

## 配置环境

- hadooptest 192.168.50.156

## 成品镜像


### docker容器配置

- windows11上的ubuntu22.04的wsl2

- docker0网桥:

```bash
mysqlBridge: 
    Subnet:192.168.50.0/24
    Gateway:192.168.50.1

```


#### 安装依赖文件

- hadooptest 192.168.50.156

1. 创建容器

```bash
# 创建容器
docker run -di --name hadooptest -v /home/lihan/sqlStudy:/home/shareFiles --net mysqlBridge --ip 192.168.50.156 ubuntu:22.04 
```

2. 进入容器

```bash
# 进入容器
docker exec -it hadooptest /bin/bash

# 安装基础工具

apt update
apt upgrade
apt install vim sudo dialog net-tools iputils-ping

```

3. 创建用户

```bash
# 创建用户

sudo useradd -m hadoop -s /bin/bash     #创建hadoop用户，并使用/bin/bash作为shell
sudo passwd hadoop                   #为hadoop用户设置密码，之后需要连续输入两次密码
# 密码 lihan
sudo adduser hadoop sudo             #为hadoop用户增加管理员权限
su - hadoop                          #切换当前用户为用户hadoop
sudo apt-get update                  #更新hadoop用户的apt,方便后面的安装

```

4. 配置SSH免密登录

```bash
sudo apt-get install openssh-server   #安装SSH server
ssh localhost                         #登陆SSH，第一次登陆输入yes
exit                                  #退出登录的ssh localhost
 
cd ~/.ssh/                            #如果没法进入该目录，执行一次ssh localhost
ssh-keygen -t rsa　　
输入完  $ ssh-keygen -t rsa　语句以后，需要连续敲击三次回车

cat ./id_rsa.pub >> ./authorized_keys     #加入授权
ssh localhost                         #此时已不需密码即可登录localhost，如果失败则可以搜索SSH免密码登录来寻求答案

```

- 报错1:
执行`ssh localhost`时报错`ssh: connect to host localhost port 22: Cannot assign requested address`

- 解决1:
ssh-server未运行，执行`sudo /etc/init.d/ssh start`启动ssh-server

5. 安装配置`jdk1.8`

- ![jdk下载](https://www.oracle.com/technetwork/java/javase/downloads)

```bash



```









