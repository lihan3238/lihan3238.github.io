---
title: Windows WSL2  下的 Docker 磁盘空间压缩
description: 在 Windows 下使用 Docker  一般在 WSL2 下，磁盘空间会被 Docker 占用很多后，即使删除了文件也不会释放空间，导致了大量占用磁盘空间问题。
slug: point_10
date: 2024-11-23 16:09:12+0800
image: imgs/avatar.png
categories:
    - techStudy
tags:
    - tech
    - docker
    - wsl
    - point
# math: 
# license: 
hidden: false
comments: true

draft: false

#links:
#  - title: 
#    description: 
#    website: 
#    image: 

#password: op

#passwordPoint: 这篇推文太_ _了
---

## 前言

这几天参加一个网安大赛，做 AI 相关的题目，要用 Docker 搭建环境运行，动不动就是几个G，我C盘直接爆炸，删了镜像和容器，发现磁盘空间没变多，就很离谱。

研究了下，发现好像是 Docker 在 WSL2 会创建一个 `ext4.vhdx` 虚拟硬盘文件，只会动态扩展变大，不会变小。可以类比想想一个橡皮泥做的袋子，往里面发放多了东西会撑大，但是拿出来的东西不会让袋子缩小，塑性形变就离谱。

## 解决

搜了搜解决方法，大概是需要手动释放空间。

1. 关闭 Docker Desktop 和 WSL2

Docker Desktop 关闭就直接关掉就行，WSL2 可以在 PowerShell 里面输入 `wsl --shutdown` 关闭。

2. 找到 `ext4.vhdx` 文件

一般在 `C:\Users\你的用户名\AppData\Local\Docker\wsl\data\ext4.vhdx`，这个文件就是 Docker 在 WSL2 里面的虚拟硬盘文件。

3. 压缩 `ext4.vhdx` 文件

管理员权限打开命令行，执行以下命令：

```powershell


diskpart

# 进入磁盘分区

select vdisk file="C:\Users\你的用户名\AppData\Local\Docker\wsl\data\ext4.vhdx"

# 查看磁盘信息

detail vdisk

# 压缩磁盘

compact vdisk

# 等他读进度条，读完了就好了

detach vdisk

exit

```
