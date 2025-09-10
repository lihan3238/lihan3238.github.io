---
title: ArchLinux VPS 学习日志
description: 用于记录购买的 ArchLinux VPS 的学习操作过程
slug: arch_vps
date: 2025-09-08 13:05:00+0800
image: imgs/avatar.png
categories:
    - techStudy
tags:
    - tech
    - archlinux
    - VPS
    - ssh
# math: 
# license: 
hidden: false
comments: true
#weight: 1  # light forward; heavy backward
draft: false

#links:
#  - title: 
#    description: 
#    website: 
#    image: 

#password: op

#passwordPoint: 这篇推文太_ _了
---

# 前言

关于折腾服务器，恐怕从大一入学就有这个想法了。无论是搞个轻薄本随身使用，远程连接服务器，还是折腾点 WEBapp ，搭建点网站公网访问，还有搭建联机游戏服务器以及自己搭建 VPN，都让我有着强烈地意愿拥有一台有公网 IP 的服务器。

前前后后，从折腾家里的公网 IP 未遂，到使用 CUC 的宿舍公网 IP （后来才知道不是一开始就有的，是 lmx 申请的），再到折腾过的腾讯云、阿里云服务器搭建[幻兽帕鲁服务器](https://lihan3238.github.io/p/palworldserver/)，我终究还是回到了 [Hostinger](https://hostinger.com.hk?REFERRALCODE=PEHLIHAN3UUE)。

之前买的一年期服务器体验很好，4 核 16GB + 50GB 以及 16 TB per month 的带宽，只要 1000 rmb （虽然对我来说还是很大的消费了 T_T ） 。

期间，我学习 linux 简单命令、golang/gin 开发、webapp 开发，也用他跑过我的简单的个人主页、hugo 博客，还有游戏服务器。总之，从中获得的知识和乐趣对得起这个价格。

这次索性搞个两年期，配置降到 2 核 8 GB 应该也够用，还是 1000 rmb 的价格。

[点我 Hostinger 获得 20% 折扣](https://hostinger.com.hk?REFERRALCODE=PEHLIHAN3UUE)

# 配置

| 项目 | 参数 |
| --- | --- |
| 服务器位置 | United States - Boston |
| 操作系统 | Arch Linux |
| 主机名 | * |
| 当前套餐 | KVM 2 |
| CPU 核心 | 2 |
| 内存 | 8 GB |
| 磁盘空间 | 100 GB |
| 带宽 | 8 TB/月 |
| 速度 | 1 Gbps | 
| 优惠价 | 1003.64 元 |

# 学习

## 包管理器

- Pacman（Primary Package Manager） 官方包管理
- AUR（Arch User Repository）       Arch 社区软件仓库
- - yay     传统的 AUR Helper
- - paru    新兴的 AUR Helper
- makepkg   手动编译安装

### pacman

[Pacman wiki](https://wiki.archlinuxcn.org/zh-sg/Pacman)

ArchLinux 的版本库里面包括：
- core-核心软件包
- extra-其他常用软件
- community-社区软件包，譬如Mysql等。
- testing-正在测试阶段，还没有正式加入源的软件包。通常软件版本比较新，但是不是非常稳定
- release-已经发布的软件包
- unstable-非正式的软件包，可能包括以前版本的软件或者测试软件

```bash
# 更新系统 update system

## 更新系统并同步仓库数据
pacman -Syu

# 安装包

pacman -S [包名]

## 一个软件包有多个版本（比如extra和testing）。你可以选择一个来安装
pacman -S extra/[包名] # 安装 指定版本(extra)的包

# 删除包

## 删除单个软件包，保留其全部已经安装的依赖关系
pacman -R [包名]

## 删除指定软件包，及其所有没有被其他已安装软件包使用的依赖关系
pacman -Rs [包名]

## 删除软件包且删除配置文件
pacman -Rn [包名]

## [常用]删除软件包及其配置文件和无用依赖
pacman -Rsn [包名]

# 查询包数据

## 查询包数据库中的软件包
pacman -Ss [包名]

## 查询已安装的软件包
pacman -Qs [包名]

## 详细信息
pacman -Si [包名]
pacman -Qi [包名]

## 获取已安装软件包所包含文件的列表
pacman -Ql [包名]

## 文件系统中某个文件是属于哪个软件包
pacman -Qo /path/to/a/file

## 罗列所有不再作为依赖的软件包(孤立orphans)
pacman -Qdt

# 其他常用命令

## 下载包而不安装它
pacman -Sw [包名]

## 安装一个 本地包（不从源里）
pacman -U /path/to/package/package_name-version.pkg.tar.gz

## 安装一个 远程包（不从源里）
pacman -U http://url/package_name-version.pkg.tar.gz

## 清理当前未被安装软件包的缓存(/var/cache/pacman/pkg)
pacman -Sc

## 完全清理包缓存
pacman -Scc

## 跳过升级软件包
IgnorePkg = 软件包名

## 跳过升级软件包组
IgnoreGroup = gnome
```

### paru

1. 安装配置

```bash
sudo pacman -S --needed git base-devel
# git：用于从 AUR 下载源码。
# base-devel：包含 make, gcc 等编译工具。

# 使用非 root 用户安装
## 克隆 paru 仓库
git clone https://aur.archlinux.org/paru.git
cd paru

## 编译并安装
makepkg -si

# 开启颜色显示

sudo vim /etc/pacman.conf
##取消注释
Color

```

## github 环境配置

### git 配置

```bash
# 安装 Git
sudo pacman -Syu git

# 设置全局用户信息
git config --global user.name "你的GitHub用户名"
git config --global user.email "你的GitHub邮箱"

# 检查配置
git config --list

## 若报错 error: cannot run less: No such file or directory fatal: unable to execute pager 'less'

## 未安装分页器 less 
sudo pacman -S less
##随后重新执行
```

### github SSH key 配置

```bash
# 生成 SSH 密钥
ssh-keygen -t ed25519 -C "你的GitHub邮箱"

# 添加 ssh-agent
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519

# 添加到 GitHub
## 登录 GitHub → Settings → SSH and GPG keys → New SSH key
## 把 ~/.ssh/id_ed25519.pub 的内容复制进去。

# 测试
ssh -T git@github.com
# 提示：Hi <你的用户名>! You've successfully authenticated... 配置成功

# clone 仓库

## ssh
git clone git@github.com:用户名/仓库名.git

## https
git clone https://github.用户名/仓库名.git


```

## golang 环境配置

### golang 安装

```bash
sudo pacman -Syu go

#配置环境变量 工作目录 GOPATH
mkdir -p ~/go/{bin,pkg,src}
echo 'export GOPATH=$HOME/go' >> ~/.bashrc
echo 'export PATH=$PATH:$GOPATH/bin' >> ~/.bashrc
source ~/.bashrc
```

## 硬链接/软链接

- **硬链接**是文件的 另一个名字，指向文件在磁盘上的 同一个 inode（即文件的实际存储位置）
- **软链接**是一个 独立的文件，内容是指向原文件路径的字符串。

| 特性           | 硬链接               | 软链接                |
| ------------ | ----------------- | ------------------ |
| 是否占用独立 inode | 否（与原文件共享 inode）   | 是（独立 inode）        |
| 是否可以跨文件系统    | 否                 | 是                  |
| 是否可以指向目录     | 否（普通用户）           | 是                  |
| 删除原文件后       | 仍可访问              | 失效                 |
| 修改内容影响原文件    | 是                 | 是（修改内容指向原文件）       |
| 查看           | `ls -li` inode 相同 | `ls -l` `->` 指向原文件 |

```bash

# 硬链接

ln a.txt b.txt

# 硬链接

ln -s [原文件或目录路径] [软链接名称]

## 查看软链接指向

readlink link_test.txt


```

## 文件权限管理

```bash
# 查看权限

ls -l 文件名或目录名

## -rw-r--r-- 1 lihan users 1024 Sep 8 22:00 test.txt
## drwxr-xr-x 2 lihan users 4096 Sep 8 22:01 mydir

## -rw-r--r--
## │ │ │ │ │
## │ │ │ │ └─ 其他用户权限
## │ │ │ └─ 用户组权限
## │ │ └─ 文件拥有者权限
## │ └─ 文件类型：- 文件，d 目录，l 链接

## 查看 ACL（Access Control List，高级权限）
getfacl 文件名

## 其他
id #显示 UID、GID 和组信息

whoami #显示当前用户名

groups #显示用户所属组


# 权限修改

## 数字方式

chmod 755 文件或目录

## 7 = rwx（4+2+1）
## 6 = rw-
## 5 = r-x
## 4 = r--

## chmod 644 test.txt  # 所有者可读写，组和其他用户只读

## 符号方式

chmod u+rwx,g+rx,o+r 文件
chmod u-w 文件

## u = 用户(owner)
## g = 组(group)
## o = 其他用户(other)
## a = all（所有人）
## + = 添加权限
## - = 移除权限
## = = 设置权限（覆盖）

## chown 改变文件拥有者
sudo chown 用户:组 文件或目录

## chgrp 改变文件所属组
sudo chgrp users test.txt

## 设置目录及其子目录/文件权限
chmod -R 755 mydir

## 新建文件继承目录组
sudo chgrp -R users mydir
sudo chmod g+s mydir
```

## cp / mv 复制剪贴

### `cp` —— 复制

```bash
cp file1 file2        # 复制文件  
cp file /dir/         # 复制到目录  
cp -r dir1 dir2       # 递归复制目录  
cp -av src dst        # 保留属性 + 显示过程  
```

### `mv` —— 移动 / 重命名

```bash
mv file /dir/         # 移动文件  
mv old new            # 重命名  
mv dir1 dir2          # 移动或改名目录  
mv -iv src dst        # 覆盖前确认 + 显示过程  
```

## tmux

`sudo pacman -S tmux`

参见[tmux 简单攻略](https://lihan3238.github.io/p/linux_tmux/)

## ssh 配置

参见[简单配置ssh免密登录](https://lihan3238.github.io/p/linuxstudy/#ssh%E5%85%8D%E5%AF%86%E7%99%BB%E5%BD%95)

### ssh 配置文件

#### 📌 SSH 配置文件区别（Arch Linux 通用）

* **ssh\_config / \~/.ssh/config**
  👉 客户端配置（你连别人）

  * 位置：`/etc/ssh/ssh_config`（全局）、`~/.ssh/config`（用户）
  * 设置内容：默认用户名、主机别名、私钥文件、代理跳板等

* **sshd\_config**
  👉 服务端配置（别人连你）

  * 位置：`/etc/ssh/sshd_config`
  * 设置内容：监听端口、是否允许 root 登录、认证方式、允许用户等

* **ssh\_config.d / sshd\_config.d**
  👉 配置片段目录（模块化管理）

  * 位置：

    * 客户端：`/etc/ssh/ssh_config.d/`
    * 服务端：`/etc/ssh/sshd_config.d/`
  * 用途：存放额外 `.conf` 配置文件，被主配置 `Include` 引入，便于分文件管理

## docker 

### docker 安装配置

```bash
# 安装

sudo pacman -Syu
sudo pacman -S docker

# 开机自启动 docker 服务

sudo systemctl enable docker
sudo systemctl start docker

# 失败的话 切 root 试试 还不行就 reboot 一下
```






# 日志

## 20250908

今天买了服务器，主要是熟悉 Arch Linux ，简单配置下 SSH 什么的，部署下我的音乐播放器。

- 配置 SSH 免密
- 配置环境：
- - `sudo pacman -S --needed git base-devel`
- - `sudo useradd -m -G wheel -s /bin/bash lihan`
- - `sudo pacman -S vim`
- - `sudo ln -s /usr/bin/vim /usr/bin/vi`
- - `sudo visudo` `%wheel ALL=(ALL) ALL`
- - `su - lihan`
- - `git clone https://aur.archlinux.org/paru.git`  `makepkg -si`
- - `sudo pacman -Syu go`
- - `mkdir -p ~/go/{bin,pkg,src}` `echo 'export GOPATH=$HOME/go' >> ~/.bashrc` `echo 'export PATH=$PATH:$GOPATH/bin' >> ~/.bashrc` `source ~/.bashrc`
- - `sudo pacman -S tmux`

## 20250910
- 继续配置环境：
- - `cp -p /root/.ssh/authorized_keys /home/lihan/authorized_keys`
- - `sudo chown lihan:lihan authorized_keys`
- - `sudo pacman -Syu` `sudo pacman -S docker`
- - `sudo systemctl enable docker` `sudo systemctl start docker`




