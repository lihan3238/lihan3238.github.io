---
title: 在Nginx服务器安装SSL证书
description: 在Nginx服务器安装SSL证书
slug: NginxSSL
date: 2023-10-18 22:13:00+0800
image: SSL.png
categories:
    - techStudy
tags:
    - web
    - SSL
    - cloudServer
    - ComputerNetworkSecurity
    - VPS
    - nginx
#weight: 1       # You can add weight to some posts to override the default sorting (date descending)
comments: true

#license: flase
#math: true
#toc: true
#style: 
#keywords:
#readingTime:
links:
  - title: 在Nginx或Tengine服务器安装SSL证书
    description: 在Nginx或Tengine服务器安装SSL证书
    website: https://help.aliyun.com/zh/ssl-certificate/user-guide/install-ssl-certificates-on-nginx-servers-or-tengine-servers
    image: https://img.alicdn.com/tfs/TB1_ZXuNcfpK1RjSZFOXXa6nFXa-32-32.ico
---
# 在Nginx服务器安装证书

在Nginx独立服务器、Nginx虚拟主机上安装证书的操作不同，请根据您的实际环境，选择对应的安装步骤。
## 在Nginx独立服务器上安装证书

### 执行以下命令，在Nginx的conf目录下创建一个用于存放证书的目录。
```bash
    cd /usr/local/nginx/conf  #进入Nginx默认配置文件目录。该目录为手动编译安装Nginx时的默认目录，如果您修改过默认安装目录或使用其他方式安装，请根据实际配置调整。
    mkdir cert  #创建证书目录，命名为cert。
```
### 将证书文件和私钥文件上传到Nginx服务器的证书目录（/usr/local/nginx/conf/cert）。
    

### 编辑Nginx配置文件nginx.conf，修改与证书相关的配置。

1. 执行以下命令，打开配置文件。
```bash
        vim /usr/local/nginx/conf/nginx.conf
```

2. 在nginx.conf中定位到server属性配置。

![nginx.conf](nginx.png)
根据如下内容进行修改。
```bash
        server {
     #HTTPS的默认访问端口443。
     #如果未在此处配置HTTPS的默认访问端口，可能会造成Nginx无法启动。
     listen 443 ssl;
     
     #填写证书绑定的域名
     server_name <yourdomain>;
 
     #填写证书文件名称
     ssl_certificate cert/<cert-file-name>.pem;
     #填写证书私钥文件名称
     ssl_certificate_key cert/<cert-file-name>.key;
 
     ssl_session_cache shared:SSL:1m;
     ssl_session_timeout 5m;
	 
     #自定义设置使用的TLS协议的类型以及加密套件（以下为配置示例，请您自行评估是否需要配置）
     #TLS协议版本越高，HTTPS通信的安全性越高，但是相较于低版本TLS协议，高版本TLS协议对浏览器的兼容性较差。
     ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
     ssl_protocols TLSv1.1 TLSv1.2 TLSv1.3;
     #表示优先使用服务端加密套件。默认开启
     ssl_prefer_server_ciphers on;
 
 
    location / {
           root html;
           index index.html index.htm;
    }
}
```

