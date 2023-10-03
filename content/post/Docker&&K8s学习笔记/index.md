---
title: Docker&&K8s学习笔记
description: 学习Docker和K8s的笔记(ubuntu)
slug: dockerStudy
date: 2023-10-03 15:41:00+0800
image: docker.png
categories:
    - techStudy
tags:
    - docker   
    - Ubuntu
    - liunx
    - k8s
#weight: 1       # You can add weight to some posts to override the default sorting (date descending)
comments: true

---
# Docker&&K8s学习笔记

## Docker
- [官网](https://hub.docker.com)
- [官方文档](https://docs.docker.com)

### Docker安装（Ubuntu22.04.3）

#### 1. 卸载旧版本

```shell
for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done
```

#### 2. 安装依赖并添加GPG密钥


```shell
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

#### 3. 添加Docker软件源仓库

```shell
echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

#### 4. 安装Docker Engine

```shell
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
// 验证安装
sudo docker run hello-world
```

#### 5. 卸载Docker Engine

```shell
//卸载软件包
sudo apt-get purge docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin docker-ce-rootless-extras
//删除所有镜像、容器和卷
sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd
```

#### 6. 设置ustc镜像源

```shell
//在/etc/docker/daemon.json中写入
{
  "registry-mirrors": ["https://docker.mirrors.ustc.edu.cn"]
}
```

### Docker常用命令

- `docker --help`：查看docker帮助

#### 1. 启动docker服务

```shell
//启动docker服务
sudo systemctl start docker
//查看docker服务状态
sudo systemctl status docker
//停止docker服务
sudo systemctl stop docker
//重启docker服务
sudo systemctl restart docker
//开机自启动docker服务
sudo systemctl enable docker
```

#### 2. 镜像相关

##### 2.1 查看镜像
  
```shell
docker images

//REPOSITORY : 镜像名称
//TAG ：镜像标签
//IMAGE ID ：镜像ID
//CREATED ：镜像创建时间
//SIZE ： 镜像大小
```
![](1.png)

##### 2.2 搜索镜像

```shell
docker search [镜像名称]

//NAME ：镜像名称
//DESCRIPTION ：镜像描述
//STARS ：镜像评价
//OFFICIAL ：是否官方
//AUTOMATED ：是否自动构建，表示该镜像由DockerHub自动构建流程创建的
```
![](2.png)

##### 2.3 拉取镜像

```shell
docker pull [镜像名称]:[标签]
//不加标签时，默认拉取latest标签
```
![](3.png)

##### 2.4 删除镜像

```shell
docker rmi [镜像ID]
//删除镜像时，要求该镜像没有被容器使用

//删除所有镜像
docker rmi `docker images -q`

```

#### 3. 容器相关

##### 3.1 查看容器

```shell
//查看正在运行中的容器
docker ps

//查看停止的容器
docker ps -f status=exited

//查看所有容器
docker ps -a

//查看最近一次创建的容器
docker ps -l
```

##### 3.2 创建与启动容器

```shell
docker run

//参数说明
//-i ：以交互模式运行容器，通常与 -t 同时使用；
//-t ：启动后进入其命令行，即为容器重新分配一个伪输入终端，通常与 -i 同时使用；
//--name ：为容器指定一个名称；
//-v ：将本地目录挂载到容器中，前一个是宿主机目录，后一个是容器内部的挂载点；
//-d ：后台运行守护式容器，创建容器后不会自动登陆容器。
//-p ：指定端口映射，前一个是宿主机端口，后一个是容器内部的映射端口，可用多个-p做多个端口映射。

(1) 交互式方式创建容器
```shell
docker run -it --name=[容器名称] [镜像名称]:[标签] /bin/bash
//创建即登录，退出即关闭
```

(2) 守护式方式创建容器
```shell
docker run -di --name=[容器名称] [镜像名称]:[标签] /bin/bash
//创建不登陆，退出不关闭

//登录容器
docker exec -it [容器名称] /bin/bash

```

##### 3.3 启动与停止容器

```shell
docker start [容器名称]
docker stop [容器名称]
```

##### 3.4 文件拷贝

```shell
//从容器拷贝到主机
docker cp [容器名称]:[容器内路径] [主机路径]
//从主机拷贝到容器
docker cp [主机路径] [容器名称]:[容器内路径]
```

##### 3.5 目录挂载
- 目录挂载是将主机的目录挂载到容器中，容器中的文件会实时同步到主机中，主机中的文件也会实时同步到容器中(共享文件夹)

```shell
docker run -di --name=[容器名称] -v [主机目录]:[容器内目录] [镜像名称]:[标签] 
//似乎文件夹名称要一样
```
##### 3.6 查看容器ip

```shell

//查看容器全部信息
docker inspect [容器名称]

//查看容器ip
docker inspect --format='{{.NetworkSettings.IPAddress}}' [容器名称]

```

##### 3.7 删除容器

```shell  
docker rm [容器名称]
//删除所有容器
docker rm `docker ps -a -q`
```











