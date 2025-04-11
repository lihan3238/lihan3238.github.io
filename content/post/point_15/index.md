---
title: Word 中公式显示不全问题
slug: point_15
date: 2025-04-11 18:15:00+0800
image: imgs/avatar.png
categories:
    - techStudy
tags:
    - computer
    - point
    - word
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

Word 文档中插入公式包含多级上下标，由于行距问题，可能出现显示不全的问题：

![wrong](imgs/wrong.png)

# 解决1（调整行距）

选中公式，并按`ctrl+1`,即可。

# 解决2（调整文本对齐方式）

如果不想调整行距，在行距足够的情况下，可以调整文本对齐方式：

1. 选中公式右键选择`段落`

![fix-1](imgs/fix-1.png)

2. 选择`中文版式`

![fix-2](imgs/fix-2.png)

3. 将`对齐方式`改为`居中`，即可

![fix-3](imgs/fix-3.png)