---
title: OpenVPN简单配置
description: 基于Debian的VPS上搭建OpenVPN服务器
slug: openvpn_01
date: 2024-05-31 13:34:00+0800
image: imgs/openvpn.png
categories:
    - techStudy
tags:
    - ComputerNetworkSecurity
    - firewall
    - VPN
    - VPS
    - Debian
# math: 
# license: 
hidden: false
comments: true

draft: false

links:
  - title: OPENVPN howto文档
    description: OpenVPN官方文档
    website: https://openvpn.net/community-resources/how-to/#determining-whether-to-use-a-routed-or-bridged-vpn
    image: https://openvpn.net/favicon.ico
  - title: OPENVPN官方github仓库
    description: OPENVPN官方github仓库
    website: https://github.com/OpenVPN/openvpn
    image: https://github.com/favicon.ico

---

## 前言

服务器一直闲着，除了搞个游戏联机服务器，偶尔泡泡程序，运行网站，想着再试试搞个VPN服务器试试，刚好注意到了OpenVPN，就试试看。

## 环境

- 服务端：Debian 11
- 测试客户端：Windows 11

## 配置

### 1. 安装OpenVPN客户端(window为例)

[下载地址](https://openvpn.net/community-downloads/)
点击下载安装即可。
![1](imgs/1.png)

### 2. OpenVPN客户端使用

![2](imgs/2.png)

将VPN服务提供者提供的.ovpn文件导入客户端，点击连接即可。

### 3. 安装OpenVPN服务端(Debian为例)

```bash
sudo apt update
sudo apt install openvpn
```

### 4. 生成主证书颁发机构 （CA） 证书和密钥

```bash
#使用easy-rsa工具生成CA证书和密钥
sudo apt install easy-rsa
# 新建文件夹保存证书
mkdir -p /etc/openvpn/certs
# 复制easy-rsa工具到/etc/openvpn/certs
cp /usr/share/easy-rsa/* /etc/openvpn/certs
# 编辑vars配置
cp ./vars.example ./vars
sudo vim ./vars
# 修改如下内容
set_var EASYRSA_REQ_COUNTRY "CN"
set_var EASYRSA_REQ_PROVINCE "Beijing"
set_var EASYRSA_REQ_CITY "Shanghai"
set_var EASYRSA_REQ_ORG "koten"
set_var EASYRSA_REQ_EMAIL "888888@qq.comm"

# 初始化CA
./easyrsa init-pki    #1、初始化，在当前目录创建PKI目录，用于存储整数

./easyrsa build-ca    #2、创建根证书，会提示设置密码，用于ca对之后生成的server和client证书签名时使用，其他提示内容直接回车即可
```

### 5. 生成server端和client端证书和密钥

```bash
# 生成server证书和密钥
./easyrsa gen-req server nopass    #1、生成server证书请求，nopass表示不设置密码
./easyrsa sign-req server server    #2、签发server证书，第一个server表示证书名称，第二个server表示证书类型

# 生成client证书和密钥
./easyrsa gen-req client1 nopass    #1、生成client证书请求，nopass表示不设置密码
./easyrsa sign-req client client1   #2、签发client证书，第一个client表示证书名称，第二个client表示证书类型
# 同理生成其他client证书和密钥

# 生成Diffie-Hellman密钥交换
./easyrsa gen-dh
```

|Filename       | 	Needed By               | 	Purpose                 | 	Secret  |
|---------------|---------------------------|---------------------------|-----------|
|ca.crt 	    |server + all clients 	    |Root CA certificate 	    |NO         |
|ca.key 	    |key signing machine only 	|Root CA key 	            |YES        |
|dh{n}.pem 	    |server only 	            |Diffie Hellman parameters 	|NO         |
|server.crt 	|server only 	            |Server Certificate 	    |NO         |
|server.key 	|server only 	            |Server Key 	            |YES        |
|client1.crt 	|client1 only 	            |Client1 Certificate 	    |NO         |
|client1.key 	|client1 only 	            |Client1 Key 	            |YES        |
|client2.crt 	|client2 only 	            |Client2 Certificate 	    |NO         |
|client2.key 	|client2 only 	            |Client2 Key 	            |YES        |
|client3.crt 	|client3 only 	            |Client3 Certificate 	    |NO         |
|client3.key 	|client3 only 	            |Client3 Key 	            |YES        |

### 6. 配置OpenVPN服务端

```bash
#示例配置文件目录:/usr/share/doc/openvpn/examples/sample-config-files
cert /etc/openvpn/certs/pki/issued/server.crt     #服务端公钥的位置
key /etc/openvpn/certs/pki/private/server.key     #服务端私钥的位置
dh /etc/openvpn/certs/pki/dh.pem                  #证书校验算法  
server 10.8.0.0 255.255.255.0                #给客户端分配的地址池
push "route 172.16.1.0 255.255.255.0"        #允许客户端访问的内网网段
ifconfig-pool-persist ipp.txt                #地址池记录文件位置，未来让openvpn客户端固定ip地址使用的
keepalive 10 120                             #存活时间，10秒ping一次，120秒如果未收到响应则视为短线
max-clients 100                              #最多允许100个客户端连接
status openvpn-status.log                    #日志位置，记录openvpn状态
log /var/log/openvpn.log                     #openvpn日志记录位置
verb 3                                       #openvpn版本
client-to-client                             #允许客户端与客户端之间通信
persist-key                                  #通过keepalive检测超时后，重新启动VPN，不重新读取
persist-tun                                  #检测超时后，重新启动VPN，一直保持tun是linkup的，否则网络会先linkdown然后再linkup
duplicate-cn                                 #客户端密钥（证书和私钥）是否可以重复
comp-lzo                                     #启动lzo数据压缩格式
```



