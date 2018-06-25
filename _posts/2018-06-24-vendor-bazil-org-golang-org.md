---
layout: post
title:  "gopkg.in和golang.org包"
date:   2018-06-24
excerpt: "gopkg.in和golang.org包"
description: "gopkg.in和golang.org包"
tag:
- golang
- raft
comments: true
---

# 以太坊包gopkg.in解析


### 以太源码不需要太过深揪
### 作为初学者应该先读懂白皮书然后
### 大致的把他所使用的包过一遍



## [bazil/fuse](https://github.com/bazil/fuse)
---

#### bazil.org/fuse 是用于编写FUSE用户空间文件系统的Go库。

[ＡＰＩ](https://godoc.org/bazil.org/fuse) 





## [golang.org]( https://golang.org/pkg/)
 ---
这个包我觉得不用怎么讲具体看[官网]( https://golang.org/pkg/)就可以了

geth 用到的包就下面这些
crypto　包密码收集常见的密码常数
net　　　Package net为网络I / O提供了一个便携式接口，包括TCP / IP，UDP，
　　　　　域名解析和Unix域套接字。	
sync　　包同步提供了基本的同步基元，例如互斥锁。
sys　　用于进行系统调用的包。
text　　用于处理文本的包。
tools　　godoc，goimports，gorename和其他工具。
## gvt
---
在这里及介绍下go的主要的包管理工具是gvt 命令 他能使包下载到vendor中
[项目地址](https://github.com/FiloSottile/gvt)
例如：
gvt fetch github.com/robertkrimen/otto 
