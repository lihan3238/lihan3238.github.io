---
title: Windows 下 ssh 连接问题
slug: point_16
date: 2025-09-18 15:32:00+0800
image: imgs/avatar.png
categories:
    - techStudy
tags:
    - computer
    - windows
    - ssh
    - firewall
#weight: 1       # You can add weight to some posts to override the default sorting (date descending)
comments: true

#license: flase
#math: true
#toc: true
#style: 
#keywords:
#readingTime:
---

# 问题

使用 ssh 连接 windows 服务器时，经常遇到一些问题，特别是配置免密登录时。

# 解决

1. 确认 ssh 服务已启动

确认在 windows 上已安装并启动了 ssh 服务

- 在 设置-系统-可选功能 中确认已安装 OpenSSH 服务器

- 在 服务 中确认 OpenSSH SSH 服务器 已启动

2. 确认防火墙已放行 22 端口

- 在 控制面板-系统和安全-Windows 防火墙-高级设置 中，确认 入站规则 中已放行 22 端口

3. 确认配置文件 sshd_config 配置正确

- 配置文件路径：`C:\ProgramData\ssh\sshd_config`
- 确认以下配置项已取消注释并配置正确：

```ini
PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys
RSAAuthentication yes
```

4. 确认用户目录下的 .ssh 目录和 authorized_keys 文件权限正确

- 目录路径：`C:\Users\你的用户名\.ssh`

- 注意 若不成功，检查`sshd_config`中是否有：

```ini
Match Group administrators
       AuthorizedKeysFile __PROGRAMDATA__/ssh/administrators_authorized_keys
```

这会覆盖默认的`AuthorizedKeysFile`配置
因此根据配置，管理员用户的公钥文件应放在`C:\ProgramData\ssh\administrators_authorized_keys`中

将公钥内容复制到`administrators_authorized_keys`文件中，重启 ssh 服务即可


