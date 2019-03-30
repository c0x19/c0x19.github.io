---
title: ssh配置
date: 2019-03-30 15:31:11
tags:
---


因为要搞ssh渗透，学习和配置一下ssh服务，环境是两个虚拟机，靶机ubuntu18.04，客户端kali

# ssh服务端配置
---

在作为服务端的虚拟机ubuntu18.04上配置ssh server

## 安装ssh server

`sudo apt-get install openssh-server`

## 查看并开启服务

安装完成后，ssh服务默认是开启的，下面是对服务的一些操作

- 查看状态：`sudo serivce ssh status`

- 开启：`sudo service ssh start`

- 停止：`sudo service ssh stop`

- 重启：`sudo service ssh restart`

## 修改端口号

防止被轻易找到默认端口22，修改端口号


1. 打开配置文件 `sudo vim /etc/ssh/sshd_config`

	找到`#port 22`这一行，在这一行下面加一句，即你要修改的端口号，如`port 9625`，保存修改

2. 重启服务 `sudo service ssh restart`


# ssh客户端访问
---

ssh命令格式：`ssh user@IP -p port`

本地测试 `ssh nik@localhost -p 9625`










