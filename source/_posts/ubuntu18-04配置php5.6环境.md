---
title: ubuntu18.04配置php5.6环境
date: 2018-07-05 17:28:52
tags:
---


# apache #

----------


## 安装 
`sudo apt-get install apache2`

## 服务使用 
- 开启 `sudo /etc/init.d/apache2 restar`
- 停止 `sudo /etc/init.d/apache2 stop`
- 重启 `sudo /etc/init.d/apache2 restar`

推荐用下面这种方法，记起来简单，通用

- 开启 `sudo service apache2 start`
- 停止 `sudo service apache2 stop`
- 重启 `sudo service apache2 restart`
- 查看状态 `sudo service apache2 status`

开启服务后，打开浏览器，进入`localhost`或`127.0.0.1`查看是否成功

# php

----------




## 安装
此处用的是php5.6

1. sudo aptitude purge \`dpkg -l | grep php| awk '{print $2}' |tr "\n" " "`
2. `sudo add-apt-repository ppa:ondrej/php`
3. `sudo apt-get update`
4. `sudo apt-get install php5.6`

安装后输入`php -v`查看是否完成

# mysql

----------

## 安装
`sudo apt-get install mysql-server mysql-client`

安装的时候竟然没有提示输入root密码，如果成功设置密码的可忽略修改密码步骤
	
先说一下服务的使用，再说修改密码

## 服务使用
- 开启 `sudo service mysql start`
- 停止 `sudo service mysql stop`
- 重启 `sudo service mysql restart`
	
可参考apache，ubuntu的服务命令套路

## 修改root密码

进入mysql默认的用户
	
`sudo vim /etc/mysql/debian.cnf`

找到这句话：password = Gt0MWsNmNv3gOW9P	复制该密码
`mysql -u debian-sys-maint -p`

然后粘贴输入该密码，进入mysql

1. `use mysql;`
2. `update user set authentication_string=PASSWORD("自定义密码") where user='root';`
3. `update user set plugin="mysql_native_password";`
4. `flush privileges;`
5. `quit`

重启服务即可



# 连接
	

----------


将php和apache连接起来

`sudo apt-get install libapache2-mod-php5.6`

对php扩展mysql

`sudo apt-get install php5.6-mysql`

php还有很多别的扩展，输入`sudo apt-get install php5.6-` 再按两下tab键，即可看到

	


	
# 安装完成


----------


为了方便使用，更改apache的根目录（写的项目放于此处）

在桌面新建web文件夹

`sudo ln -s /home/nik/Desktop/web /var/www/html`

此时在web目录中创建cwd.php，添加如下

    <?php  
    	echo "MFFL";
    ?> 

在浏览器输入`localhost/web/cwd.php`，检查成功