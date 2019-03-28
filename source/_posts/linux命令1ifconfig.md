---
title: linux命令-1-ifconfig
date: 2019-03-28 13:30:13
tags:
---

ifconfig可以查看并修改网络设备参数

# 命令格式 #

`ifconfig [网络设备] [参数]`

# 常用功能参数 #

- up：启动指定网络设备/网卡 `ifconfig eth0 up`

- down: 关闭指定网络设备/网卡 `ifconfig eth0 down`

- arp： 设置指定网卡是否支持ARP协议 
	- 启用ARP协议 `ifconfig eth0 arp`
	- 关闭ARP协议 `ifconfig eth0 -arp`

- -a： 显示全部接口信息 `ifconfig -a`

- 配置ip地址： 给eth0网卡设置ip为192.168.1.100 `ifconfig eth0 192.168.1.100`
