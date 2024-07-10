---
title: tmux 简单攻略
description: 用于分屏、多人共享命令行、会话与窗口分离
slug: linux_tmux
date: 2024-07-10 08:59:00+0800
image: imgs/tmux.png
categories:
    - techStudy
tags:
    - linux
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

## 会话操作

- `Ctrl+b ?` # 显示快捷键帮助

```bash

tmux # 启动tmux并一个新的会话

# 新建会话
tmux new -s <会话名称>
Ctrl+b c 

# 分离会话
tmux detach
Ctrl+b d

# 查看会话
tmux ls
Ctrl+b s

# 连接会话
tmux attach -t <会话名称> (tmux a -t <会话名称>)

# 杀死会话
tmux kill-session -t <会话名称> 

# 重命名会话
tmux rename-session -t <旧会话名称> <新会话名称>
Ctrl+b $ # 重命名当前会话

```

## 窗格操作

- 本质上是一个会话中的多个窗格，不是开启一个新的会话

```bash
# 划分窗格

## 上下划分
Ctrl+b "
tmux split-window 

## 左右划分
Ctrl+b %
tmux split-window -h

# 切换光标

Ctrl+b 方向键
Ctrl+b ;    # 切换到上一个窗格
Ctrl+b o    # 切换到下一个窗格
tmux select-pane -U # 上
tmux select-pane -D # 下
tmux select-pane -L # 左
tmux select-pane -R # 右

# 交换窗格
tmux swap-pane -U # 上移当前窗格
tmux swap-pane -D # 下移当前窗格

# 关闭窗格
Ctrl+b x

# 拆分当前窗格为独立窗格
Ctrl+b !

# 全屏/取消全屏
Ctrl+b z

# 调整窗格大小
Ctrl+b Ctrl+方向键

# 显示窗格编号
Ctrl+b q