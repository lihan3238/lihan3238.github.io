---
title: 基于ActivityPub的实时想法展板搭建(待完成)
description: 使用 ActivityPub 协议搭建一个实时想法展板。
slug: activitypub_01
date: 2024-12-24 09:35:00+0800
image: imgs/avatar.png
categories:
    - techStudy
tags:
    - tech
    - ActivityPub
    - Blog
    - CloudServer
# math: 
# license: 
hidden: false
comments: true

draft: false

links:
 - title: 吕楪的实时动态
   description: 参考
   website: https://toot.irithys.com/@thy?ref=irithys.com
   image: https://toot.irithys.com/fileserver/01Q45X8Y3GKDNGX90WABCSX34Y/attachment/original/01DH17C7VCG7MA99HZVKR2XXSC.png

 - title: ActivityPub Guide
   description: ActivityPub 官方文档
   website: https://socialhub.activitypub.rocks/pub/guide-for-new-activitypub-implementers
   image: https://activitypub.rocks/static/images/ActivityPub-logo.svg

 - title: Mastodon
   description: Mastodon 是一个开源的去中心化的社交网络
   website: https://mastodon.social/
   image: https://joinmastodon.org/_next/static/media/favicon-32x32.3a702fe9.png

#password: op

#passwordPoint: 这篇推文太_ _了
---

## 前言

一直就有中博客上设置一个实时动态页面的想法，记录一些突然蹦进脑袋里的想法。

最开始的时候，专门搞了一个文章页面，用评论区来记录，但是这样总怪怪的，就取消了。

大改博客后，我把所有使用了 `musing` tag 的文章作为 `随想` 在单独的随想页面展示，这样虽然emmm 齐整了点，但是还是不够实时。

看到 [吕楪](https://toot.irithys.com/@thy?ref=irithys.com) 的实时动态，发现这正是我需要的效果，顺着发现了 `ActivityPub` 协议，这个协议还可以让我们的博客和其他的社交平台进行去中心化交互，这样就可以实现一个实时动态了。

## 题外

诚然，我清楚地知晓，开发实时动态页面，抑或是其他什么，不过是在转移自己的注意力罢了。

有人会把个人的困顿愁虑用虚无缥缈的宏大叙事来掩饰，称这种忧伤不过是小布尔乔亚的无病呻吟，我虽已不这般骗自己，但料想自己和他们也没什么两样吧。

嘴上一直说自己运气好，其实一直不愿意把心放在这不可控的玄幻之物上。不过在她这里，我的运气似乎从来没有灵光。

就是忧伤，

没什么，就是忧伤

## ActivityPub

根据 [how-to](https://blog.joinmastodon.org/2018/06/how-to-implement-a-basic-activitypub-server/) 大概了解了 `ActivityPub` 的基本原理。

似乎开发这样一个留言板是一个浩大的工程，基于一个底层的协议，需要自己实现很多东西，精力还是放在完善毕设先，等以后有时间再来搞这个。

又注意到 `Mastodon` 这个开源的社交网络，是一种基于 `ActivityPub` 协议的实现，索性直接加个页面链接过去吧。

