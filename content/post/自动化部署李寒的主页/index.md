---
title: 自动化部署李寒的主页
description: 在服务器上自动化部署李寒的主页
slug: lihan_auto
date: 2024-03-29 19:00:19+0800
image: imgs/avatar.png
categories:
    - techStudy
tags:
    - tech
    - docker
    - nginx
    - linux
    - Debian
# math: 
# license: 
hidden: false
comments: true

draft: false

---

## 前言

搞了个VPS,想着用nginx部署一个主页,上面是李寒的小窝等我的网站的连接,除了李寒的小窝以外的网站使用gin运行，
考虑到VPS可能会更换,搞个Dockfile,方便以后部署.

## 环境

- Debian 11
- Docker version 26.0.0, build 2ae903e
- nginx:latest

## 服务器初始化

### 安装docker

```bash

# 安装docker

## 卸载旧版本
for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done

## Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

## Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

```

### 依赖环境配置

```bash

# 新建用户

sudo adduser lihan
sudo usermod -aG sudo lihan
su - lihan
sudo -l
cd

mkdir share_file
cd share_file
git clone https://github.com/lihan3238/okx_test.git
git clone https://github.com/lihan3238/handong_music.git
git clone https://github.com/lihan3238/lihan_nginx.git

```

## 网站运行上线


```bash

# gin网页

wget https://go.dev/dl/go1.22.1.linux-amd64.tar.gz
sudo rm -rf /usr/local/go && sudo tar -C /usr/local -xzf go1.22.1.linux-amd64.tar.gz
sudo rm -rf go1.22.1.linux-amd64.tar.gz

sudo vim ~/.bashrc


export PATH=$PATH:/usr/local/go/bin


source ~/.bashrc

go version

sudo vim /etc/systemd/system/handong_music.service

[Unit]
Description=Handong Music Gin App
After=network.target

[Service]
ExecStart=/usr/local/go/bin/go run /home/lihan/share_files/handong_music/main.go
Restart=always
WorkingDirectory=/home/lihan/share_files/handong_music
User=lihan
Environment=GIN_MODE=release

[Install]
WantedBy=multi-user.target


sudo vim /etc/systemd/system/okx_lihan.service

[Unit]
Description=
After=network.target

[Service]
ExecStart=/usr/local/go/bin/go run /home/lihan/share_files/okx_test/main.go
Restart=always
WorkingDirectory=/home/lihan/share_files/okx_test
User=lihan
Environment=GIN_MODE=release

[Install]
WantedBy=multi-user.target

## 重新加载配置文件
sudo systemctl daemon-reload
# 启动
sudo systemctl start handong_music
sudo systemctl start okx_lihan
# 开机自启
sudo systemctl enable handong_music
sudo systemctl enable okx_lihan
# 查看状态
sudo systemctl status handong_music
sudo systemctl status okx_lihan


# nginx 主页

sudo docker pull nginx

sudo docker run --name lihan_nginx \
  --mount type=bind,source=/home/lihan/share_files/lihan_nginx/html,target=/usr/share/nginx/html \
  --mount type=bind,source=/home/lihan/share_files/lihan_nginx/conf/nginx.conf,target=/etc/nginx/nginx.conf \
  --mount type=bind,source=/home/lihan/share_files/lihan_nginx/conf.d,target=/etc/nginx/conf.d \
  --mount type=bind,source=/home/lihan/share_files/lihan_nginx/logs,target=/var/log/nginx \
  -p 80:80 -p 443:443 \
  -d nginx 

```












