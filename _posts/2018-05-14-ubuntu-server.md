---
layout: post
title:  "ubuntu优化"
date:   2018-05-10
excerpt: "ubuntu搭建区块链"
description: "ubuntu16.4优化"
tag:
- ubuntu 
comments: true
---
一，root
1.  安装 open ssh：

sudo apt-get install openssh-server

修改 root 密码

sudo passwd root

2. 以其他账户登录，通过 sudo nano 修改 /etc/ssh/sshd_config :

xxx@ubuntu:~$ su - root   #切换到root账户

Password:                           #输入第二步修改的root密码

root@ubuntu:~# vi /etc/ssh/sshd_config

4
3. 找到下面这段代码，注释掉 #PermitRootLogin without-password，添加 PermitRootLogin yes

# Authentication: 

LoginGraceTime 120 

#PermitRootLogin prohibit-password

PermitRootLogin yes 

StrictModes yes

5
4. 重启 ssh  服务

root@ubuntu:~# sudo service ssh restart 

root@ubuntu:~#


网关
1、vi /etc/network/interfaces

添加内容：

auto eth0
iface eth0 inet static
address 192.168.8.100    
netmask 255.255.255.0
gateway 192.168.8.2
dns-nameserver 114.114.114.114


dns-nameserver 114.114.114.114这句一定需要有，

因为以前是DHCP解析，所以会自动分配DNS 服务器地址。

而一旦设置为静态IP后就没有自动获取到DNS服务器了，需要自己设置一个

设置完重启电脑后，/etc/resolv.conf 文件中会自动添加 nameserver 119.29.29.29

(或者nameserver 8.8.8.8)可以根据访问速度，选择合适的公共DNS 






2、重启网络：sudo /etc/init.d/networking restart


修改主机名：
重启后生效
vi /etc/hostname
----------------------------------------------------------

 redhat/centos上永久修改：

 [root@localhost ~]# cat /etc/sysconfig/network

    NETWORKING=yes

    HOSTNAME=localhost.localdomain

    GATEWAY=192.168.10.1

    修改network的HOSTNAME项。点前面是主机名，点后面是域名。没有点就是主机名。

    [root@localhost ~]# vi /etc/sysconfig/network

    NETWORKING=yes

    NETWORKING_IPV6=no

    HOSTNAME=gdbk

    这个是永久修改，重启后生效。

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------

二、临时修改主机名的方法 重启后失效：

 显示主机名：

    zhouhh@zzhh64:~$ hostname

    zhh64

 

 修改主机名：

    zhouhh@zzhh64:~$ sudo hostname zzofs

    zhouhh@zzhh64:~$ hostname

    zzofs

三，防火墙
1、关闭ubuntu的防火墙

  ufw disable
1
2
2开启防火墙

 ufw enable
1
2
3、卸载了iptables

   apt-get remove iptables
1
2
4、关闭ubuntu中的防火墙的其余命令

    iptables -P INPUT ACCEPT
    iptables -P FORWARD ACCEPT
    iptables -P OUTPUT ACCEPT
    iptables -F


四，ubuntu16.04 server 开启 ssh
sudo apt-get install openssh-server
检查是否安装成功
sudo ps -e |grep ssh
开启ssh服务
sudo service ssh start　
修改ssh可以使用root登陆
sudo vi /etc/ssh/sshd_config
找到PermitRootLogin prohibit-password一行，改为PermitRootLogin yes
保存，并重起ssh
sudo service ssh restart
  1，ubuntu下安装、启动和卸载SSH
http://blog.csdn.net/swuteresa/article/details/9377169

没有make命令
apt-get install build-essential 

ubuntu安装goland
官网现在包
解压到
目录
配置环境变量 




NAT模式下 宿主机与虚拟机不能Ping通的解决方式
http://blog.csdn.net/jin5203344/article/details/61916713 


解决Ubuntu中update的问题(Reading package lists... Error!)
http://blog.csdn.net/wuyilun2013/article/details/43151191
sudo rm /var/lib/apt/lists/* -vf
sudo apt-get update