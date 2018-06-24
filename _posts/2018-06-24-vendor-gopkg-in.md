---
layout: post
title:  "以太坊包gopkg.in解析"
date:   2018-06-24
excerpt: "以太坊包gopkg.in解析"
description: "以太坊包gopkg.in解析"
tag:
- golang
- raft
comments: true
---

# 以太坊包gopkg.in解析


### 以太源码不需要太过深揪
### 作为初学者应该先读懂白皮书然后
### 大致的把他所使用的包过一遍



## gopkg.in
 
go中除了go get github 包还可以 
go get [gopkg.in](http://labix.org/gocheck)

gopkg.in和github差不多但是
使用gopkg.in URL更清晰，更短，重定向到使用浏览器打开的godoc.org中的包文档，
处理用于版本控制的git分支和标记，最重要的是鼓励采用稳定的版本化包API 。

这是他的格式
gopkg.in/pkg.v3      重定向→ github.com/go-pkg/pkg (branch/tag v3, v3.N, or v3.N.M)
gopkg.in/user/pkg.v3   重定向→ github.com/user/pkg   (branch/tag v3, v3.N, or v3.N.M)

----

### [check.v1](https://github.com/immerselearning/check.v1)
　

-----
### [gopkg.in/check](https://github.com/fatih/set)
Set是Go（Golang）中基本的简单的基于哈希的Set数据结构实现。

Set提供通用集数据结构的线程安全和非线程安全实现。线程安全涵盖了一套的所有操作。多组的操作是一致的，因为每组使用的元素在操作的开始和结束之间恰好有一个时间点是有效的。
因为它是线程安全的，所以可以在你的goroutine中同时使用它。

-----
### [karalabe/cookiejar](https://github.com/karalabe/cookiejar)

CookieJar是常见算法，数据结构和库扩展的一个小集合，被认为在计算某个或那些点竞争时非常方便。

这个工具箱暂时正在进行中。它可能是缺乏的，它可能会在提交之间发生巨大的变化（尽管没有做出任何努力）。欢迎您使用它，但它是你的头在线:)

---
### [natefinch/npipe](https://github.com/natefinch/npipe)
npipe提供了一个基于stdlib的net包的接口，具有Dial，Listen和Accept功能，以及net.Conn和net.Listener的相关实现。它通过连接支持rpc。

----
### [olebedev/go-duktape](olebedev/go-duktape)
Duktape是一个薄的，可嵌入的JavaScript引擎。大多数API都已经实现

---
### [go-sourcemap/sourcemap](https://github.com/go-sourcemap/sourcemap/tree/v1.0.5/base64vlq)
base64的编码和解码

---
### [urfave/cli](https://github.com/urfave/cli)
cli命令框架包
