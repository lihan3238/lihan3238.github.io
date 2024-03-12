---
title: stack主题版本更新后hugo版本依赖问题
slug: point_8
date: 2024-03-12 14:38:00+0800
image: stack.png
categories:
    - techStudy
tags:
    - computer
    - point
    - hugo
    - golang
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

![err](err.png)

hugo主题stack版本更新到v3.24.0后，搭建网站时会出现如图报错。

# 解决

stack v3.24.0 版本需要 hugo 0.123.8以上版本。
更新 hugo 版本即可解决。

# 注意

使用![github部署hugo模板](https://lihan3238.github.io/p/hugowebtest/#13-github%E9%83%A8%E7%BD%B2hugo%E6%A8%A1%E6%9D%BF)时，也会出现同样的问题，需要修改`.github\workflows\hugo.yml`文件中的hugo版本。

```yml
jobs:
  # Build job
  build:
    runs-on: ubuntu-latest
    env:
      HUGO_VERSION: 0.123.8
```