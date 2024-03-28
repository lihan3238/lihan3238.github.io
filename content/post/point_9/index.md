---
title: scp等传输文件后权限问题
slug: point_9
date: 2024-03-28 11:38:00+0800
image: scp.png
categories:
    - techStudy
tags:
    - computer
    - point
    - scp
    - linux
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

使用`scp`等命令传输文件后，文件权限不正确，可能导致依赖的程序无法正常读写文件。

# 解决

`chmod`命令可以修改文件权限，我的评价是直接 777