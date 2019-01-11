---
title: ubuntu18.04装pwndbg
date: 2018-07-09 16:33:17
tags: pwndbg
---

以前用的是peda插件，今天换成pwndgb，功能更多，而且界面更好看

参考 [https://github.com/pwndbg/pwndbg](https://github.com/pwndbg/pwndbg) 

# 安装
----------

**按照网站的教程装就好了：**

1. `git clone https://github.com/pwndbg/pwndbg`
2. `cd pwndbg`
3. `sudo ./setup.sh`

第三步对比官网加了sudo，因为发现最后有权限问题

**禁用peda**

参考peda的安装步骤：

1. `git clone https://github.com/longld/peda.git ~/peda`
2. `echo "source ~/peda/peda.py" >> ~/.gdbinit`

第二步在~/.gdbinit中加了开启peda脚本

可以打开pwndbg的setup.sh，在最后面发现也是在~/.gdbinit中加了开启pwndbg脚本，

即：`echo "source $PWD/gdbinit.py" >> ~/.gdbinit`


所以打开~/.gdbinit删除peda那句话就好了

同理，**peda和pwndbg的切换**就是在~/.gdbinit中切换语句



# 简单使用
----------

**界面**

从上到下为寄存器、代码、栈、函数回溯关系

界面更加美观

* 调用函数时，会显示函数的参数
* 条件跳转语句的前面，会显示对号或叉号来表示是否跳转
* 栈中会标出esp和ebp

**命令**

* stack：查看栈，后面可加数字来规定查看的大小，如`stack`或`stack 20`
* vmmap：虚拟地址映射，可查看libc等基址
* telescope：查看内存，`telescope $rsp 20`等价于`stack 20`