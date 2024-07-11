--- 
title: Git 详细攻略
description: Git 概念 操作 配置挺多 索性开个新帖子专门慢慢记录
slug: git_lihan
date: 2024-07-11 11:35:00+0800
image: imgs/git.png
categories:
    - techStudy
tags:
    - Git
    - GitHub
    - Gitlab
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

## tips

### 1. Git 本地仓库绑定多个(Github/Gitlab)远程仓库

- [git 本地仓库同时推送到多个远程仓库](https://blog.csdn.net/fb_help/article/details/83826352)

进一步学习了 Git 的 `本地仓库` 和 `远程仓库`的关系后，理解了 **本地仓库和远程仓库之间的关系通过一组远程引用（remote references）来管理。远程引用是一组指向远程仓库中提交对象的指针（例如分支和标签）。可以为一个本地仓库绑定多个远程仓库。** 
于是由 `lihan3238` 将自己的本地仓库同时绑定了两个远程仓库，每次将 [项目工作仓库(GitHub)](https://github.com/lihan3238/Network-Security-Comprehensive-Practice) 的更新拉取到本地仓库，并将更新推送到 [项目主仓库(Gitlab)]( https://git.cuc.edu.cn/ccs/2024-summer-cp/Network-Security-Comprehensive-Practice) 中。

```bash

# 绑定远程仓库-1
git remote add cuc https://git.cuc.edu.cn/ccs/2024-summer-cp/Network-Security-Comprehensive-Practice.git

## 此时使用 git remote -v 查看绑定情况
## cuc     https://git.cuc.edu.cn/ccs/2024-summer-cp/Network-Security-Comprehensive-Practice.git (fetch)
## cuc     https://git.cuc.edu.cn/ccs/2024-summer-cp/Network-Security-Comprehensive-Practice.git (push)
## origin  https://github.com/lihan3238/Network-Security-Comprehensive-Practice.git (fetch)
## origin  https://github.com/lihan3238/Network-Security-Comprehensive-Practice.git (push)

## 每次使用 git pull origin main 拉取工作仓库的更新，git push cuc main 推送到主仓库

# 绑定远程仓库-2
git remote set-url --add origin https://git.cuc.edu.cn/ccs/2024-summer-cp/Network-Security-Comprehensive-Practice.git

## 此时使用 git remote -v 查看绑定情况
## origin  https://github.com/lihan3238/Network-Security-Comprehensive-Practice.git (fetch)
## origin  https://github.com/lihan3238/Network-Security-Comprehensive-Practice.git (push)
## origin  https://git.cuc.edu.cn/ccs/2024-summer-cp/Network-Security-Comprehensive-Practice.git (push)
## 注意到 origin 的 push 地址有两个，分别是 github 和 gitlab 的地址，这样每次 git push origin main 时，会将更新推送到两个远程仓库，但是 pull 时只会从工作仓库拉取

```

注意：

```bash
git push [远程仓库名] [分支名] 
# 缺省 [远程仓库名] 时，默认为 origin，缺省 [分支名] 时，默认为当前分支
git push [远程仓库名] --all
# 推送所有分支
```
