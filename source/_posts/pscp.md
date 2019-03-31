---
title: pscp
date: 2019-03-31 16:58:50
tags:
---

pscp类似scp，但扩展了对windows的支持，主要用来windows与linux传输文件

# 文件交互 #

- windows=>linux：`pscp 传输的文件 用户@IP地址:目录`

	如：`pscp D:\test\test.txt root@192.168.1.100:/home/test`

- linux=>windos：`pscp 用户@IP地址:/文件 本地地址`

	如：`pscp root@192.168.1.100:/home/test/test.txt D:\test`

# 文件夹 #

类似文件交互，加上`-r`参数即可