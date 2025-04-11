---
title: Word 转 PDF 页眉报错问题
slug: point_14
date: 2025-04-11 18:10:00+0800
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

Word 文档设置页眉后，转为 PDF 时，页眉显示错误：

```
Error! Use the Home tab to apply 标题 3 to the text that you want to appear here.
```

# 解决

页眉中选择了 Word 的样式，由于一些 BUG 转为 PDF 时，样式的变量名出现问题，无法选择正确的样式。

更改页眉中的样式名即可:

1. 选择页眉

![fix-1](fix-1.png)

2. 右键选择`切换域代码`

![fix-2](fix-2.png)

3. 将代码中的`标题 1`改为`1`，再次右键选择`切换域代码`，即可

![fix-3](fix-3.png)