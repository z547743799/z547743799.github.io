---
layout: post
title:  "ubuntu-安装MySQL"
date:   2018-05-22
excerpt: "ubuntu-安装MySQL"
description: "ubuntu-安装MySQL"
tag:
- Msyql
- ubuntu
comments: true
---



简单分享Ubuntu 16.04下安装MySQL的过程。

首先执行下面三条命令：

sudo apt-get install mysql-server

sudo apt install mysql-client

sudo apt install libmysqlclient-dev

安装成功后可以通过下面的命令测试是否安装成功：

sudo netstat -tap | grep mysql

出现如下信息证明安装成功：



可以通过如下命令进入MySQL服务：

mysql -uroot -p你的密码

现在设置mysql允许远程访问，首先编辑文件/etc/mysql/mysql.conf.d/mysqld.cnf：

sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf

注释掉bind-address = 127.0.0.1：



保存退出，然后进入mysql服务，执行授权命令：

grant all on *.* to root@'%' identified by '你的密码' with grant option;

flush privileges;

然后执行quit命令退出mysql服务，执行如下命令重启mysql：

service mysql restart