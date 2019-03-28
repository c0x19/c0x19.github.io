---
title: linux命令3iwlist
date: 2019-03-28 15:06:28
tags:
---

iwlist命令利用无线网卡进行无线AP的检测与取得相关的数据

AP：WirelessAcessPoint，无线访问接入点，就是传统网络中的HUB，起到中继和桥接的作用。无线路由器就是无线AP+路由功能

#功能#


## 扫描可用的无线网络## 

1. 找到可用的无线网卡 `iwconfig`

2. 一般无线网卡为wlan0，激活开启网卡 `ifconfig wlan0 up`

3. 扫描网络，`iwlist wlan0 scan`，其中scan为scanning简写，两种写法都可以


## 显示频道信息 ##

`iwlist wlan0 frequen`