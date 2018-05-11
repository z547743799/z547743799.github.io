---
layout: post
title:  "ubuntu16.4安装官方reactjs"
date:   2018-05-10
excerpt: "ubuntu安装官方reactjs"
description: "ubuntu安装官方react"
project: true
tag:
- react 
- nodejs
- ubuntu
comments: true
---

#### [reactjs官网](https://reactjs.or	g/)
![reactjs](http://p8am46xs9.bkt.clouddn.com/react%E5%AE%98%E7%BD%91.png)
##### npm install 项目中一定要有 package.json 和package-lock.json



#### 清理缓存
     npm cache clean --force
     
 #### ubuntu 出错大部分是权限问题
  ![reactjs](http://p8am46xs9.bkt.clouddn.com/npm%20start%20%E6%9D%83%E9%99%90%E9%97%AE%E9%A2%98.png)
     npm start要权限
     sudo npm start
     也要权限
     sudo npm install
     实在不行转 ru root 
     如果没有设置root密码
        
        sudo passwd

![](http://images2017.cnblogs.com/blog/1154148/201712/1154148-20171201103242867-114669375.png)
  
###### 出现的黄色部分是警告
   ![reactjs](http://p8am46xs9.bkt.clouddn.com/npm%E5%AE%89%E8%A3%85%E5%8F%AA%E6%98%AF%E8%AD%A6%E5%91%8A.png)
 ##### npm  6.0降级5.6
npm i -g npm@5.6.0     //基本没用