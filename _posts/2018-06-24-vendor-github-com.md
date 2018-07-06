---
layout: post
title:  "以太坊包github.com解析"
date:   2018-06-24
excerpt: "以太坊包github.com解析"
description: "以太坊包github.com解析"
tag:
- eth
comments: true
---


# 以太坊包github.com解析


### 以太源码不需要太过深揪
### 作为初学者应该先读懂白皮书然后
### 大致的把他所使用的包过一遍



## github.com
---
### Azure

Azure是由Microsoft创建的云计算服务，用于通过Microsoft管理的数据中心的全球网络构建，测试，部署和管理应用程序和服务。

### azure-sdk-for-go
提供用于管理和使用Azure服务的Go软件包
[azure-sdk-for-go](https://github.com/Azure/azure-sdk-for-go)


### azure-storage-go
用于Go的Microsoft Azure存储SDK允许您构建利用Azure可扩展云存储的应用程序。
[azure-storage-go](https://github.com/Azure/azure-storage-blob-go)

### go-autorest
软件包go-autorest提供了一个HTTP请求客户端，可用于Autorest生成的API客户端软件包。
[go-autorest](https://github.com/Azure/go-autorest)
---
btcsuite/btcd
 编写的替代完整节点比特币实现
[btcsuite/btcd](https://github.com/btcsuite/btcd)

---
### cespare/cp
cp是用于复制文件和目录的小型Go软件包。
 [cespare/cp](https://github.com/cespare/cp) 
---

### davecgh/go-spew
Go-spew为Go数据结构实现了一个深度漂亮的打印机，以帮助进行调试。
 [davecgh/go-spew](https://github.com/davecgh/go-spew)
 
---

### dgrijalva / jwt-go
go 实现的JSON网络令牌
[什么是JSON Web Token？](https://jwt.io/introduction/)

[dgrijalva / jwt-go](https://github.com/dgrijalva/jwt-go)

---
### Shopify/docker
该reexec包促进了docker二进制文件的busybox风格reexec，因为使用Go的分叉限制，我们需要该文件。处理程序可以注册一个名称，二进制的exec的argv 0将用于查找和执行自定义init路径。
[Shopify/docker](https://github.com/Shopify/docker/tree/master/pkg/reexec)
---

### edsrzf / mmap-go
mmap-go是Go编程语言的便携式mmap软件包。它已经在Linux（386，amd64），OS X和Windows（386）上测试过。它也应该适用于其他类Unix平台，但尚未经过测试 
[edsrzf / mmap-go](https://github.com/edsrzf/mmap-go)

---
### elastic/gosigar
 sigar是sigar API的golang实现 。Go版本的sigar有一个非常类似的界面，但是在纯go / cgo中是从头开始编写的，而不是用于libsigar的cgo绑定。
[elastic/gosigar](https://github.com/elastic/gosigar)

---
### ethereum/ethash
挖矿工作量算法证明与实现
[ethereum/ethash](https://github.com/ethereum/ethash/tree/master/src/libethash)

---
### fatih/color
Color允许您使用Go（Golang）中的ANSI Escape Codes使用彩色输出。它也支持Windows！API可以以多种方式使用，挑选适合您的API。
[fatih/color](https://github.com/)

---
memsize计算对象图的大小
[fjl/memsize](https://github.com/fjl/memsize)

---
### gizak/termui
termui是一个跨平台，易于编译和完全可定制的终端仪表板。
[gizak/termui](https://github.com/gizak/termui)
### fjl/memsize

---
### go-ole/go-ole
golang的win32 ole实现
Win32 :: OLE模块可让您自动访问Windows应用程序，如Word，Excel，Access，Rational Rose，Lotus Notes和其他许多应用程序。这意味着您的Perl脚本可以利用这些应用程序的功能，数据和方法。其他Win32 perl模块建立在此模块上（例如DBD :: ADO，Access数据库的DBI驱动程序）。
[go-ole/go-ole](https://github.com/go-ole/go-ole)
---
### go-stack/stack
包栈实现实用程序来捕获，操作和格式化调用堆栈。它提供了比包运行时更简单的API。
[go-stack/stack](https://github.com/go-stack/stack)

---
### golang/protobuf
支持谷歌缓冲区的数据交互格式
协议缓冲区是以高效但可扩展的格式编码结构化数据的一种方式。谷歌几乎所有的内部RPC协议和文件格式都使用Protocol Buffers。
[golang/protobuf](https://github.com/golang/protobuf)


### golang/snappy
Go编程语言中的Snappy压缩格式
[golang/snappy](https://github.com/golang/snappy)

---
### hashicorp/golang-lru
这提供了lru实现固定大小的线程安全LRU缓存的包。它基于Groupcache中的缓存。
[hashicorp/golang-lru](https://github.com/hashicorp/golang-lru)

---
### huin/goupnp
goupnp是Go的UPnP客户端库
### UPnP简介
upnp是 universal plug and play，即：即插即用设备，可以当作是一个相对复杂的网络协议，毕竟它包含了很多其他的网络协议，如：ip（设备寻址），tcp、udp（数据打包发送）、http（数据传递格式）等。

upnp可以扩展，也就是说你还可以在启动加入其他的协议，比如：传递数据时，http协议再包一层json协议，或者数据传递使用xml协议来传递等等。

upnp之所以强大，感觉很大一个原因是基于互联网，这样对等设备可以通过互联网自由交互，也就是说任何可以联网的设备都可以使用upnp协议。
[huin/goupnp](https://github.com/huin/goupnp)

---

### influxdata/influxdb
开源时间序列数据库
InfluxDB是一个开源的时间序列数据库， 没有外部依赖性。记录指标，事件和执行分析很有用。
[influxdata/influxdb](https://github.com/influxdata/influxdb)

### jackpal/go-nat-pmp
go-NAT-PMP
用于NAT-PMP网络协议的Go语言客户端，用于端口映射并发现防火墙的外部IP地址。
NAT-PMP由Apple品牌路由器和Tomato和DD-WRT等开源路由器支持。
[jackpal/go-nat-pmp](https://github.com/jackpal/go-nat-pmp)

---
### julienschmidt/httprouter
HttpRouter是一种轻质高性能的HTTP请求路由器（也称为多路转换器或只是多路复用器用于短）转到。

与Go的软件包的默认多路复用器相反net/http，该路由器支持路由模式中的变量并与请求方法匹配。它也可以更好地扩展。

该路由器针对高性能和小内存占用情况进行了优化。即使有很长的路径和大量的路线，它也能很好地扩展。压缩动态树（基树）结构用于高效匹配。
[julienschmidt/httprouter](https://github.com/julienschmidt/httprouter)


---
### karalabe/hid
Gopher接口设备（USB HID）
该hid软件包是用于访问USB人机界面设备（HID）并与之通信的跨平台库。gousb对于设备支持这种更加合理的操作模式（例如输入设备，硬件加密钱包）的用例，这是一种替代方案。

该软件包hidapi用于直接访问特定于操作系统的USB HID API，而不是使用低级USB构造，这可能会在某些平台上出现许可问题。在Linux上，软件包也包装在内libusb。这两种依赖关系都直接进入存储库并使用CGO进行hid打包，从而使软件包自成一体并且可以继续使用。

目前支持的平台是Linux，macOS和Windows（不包括为Android和iOS指定的约束，以允许跨平台项目顺利销售）。

[karalabe/hid](https://github.com/karalabe/hid)

---
### maruel/panicparse
解析恐慌堆栈跟踪，使用类似堆栈跟踪的密集化和重复数据删除。在严重并行化的过程中帮助调试崩溃和死锁。
[maruel/panicparse](https://github.com/maruel/panicparse)


---
### mattn/go-colorable
这个软件包可以处理windows上ansi颜色的转义序列。
[mattn/go-colorable](https://github.com/mattn/go-colorable)

### mattn/go-isatty

isatty，函数名。主要功能是检查设备类型 ， 判断文件描述词是否是为终端机。
[isatty](https://baike.baidu.com/item/isatty/6392452)

[mattn/go-isatty](https://github.com/mattn/go-isatty)

### mattn/go-runewidth
提供获取字符或字符串的固定宽度的函数。
  
[mattn/go-runewidth](https://github.com/mattn/go-runewidth)

---
### mitchellh/go-wordwrap
go-wordwrap（Golang包：）wordwrap是Go的一个包，可以自动将单词包装成多行。主要的用例是格式化CLI输出，但是当然换行是一个非常有用的事情
[mitchellh/go-wordwrap](https://github.com/mitchellh/go-wordwrap)

---
### naoina/go-stringutil
Go的更快的字符串实用程序实现
[naoina/go-stringutil](https://github.com/naoina/go-stringutil)

### naoina/toml
TOML解析器和Golang编码器库
[TOML](https://github.com/toml-lang/toml)
[naoina/toml](https://github.com/naoina/toml)

---
### nsf/termbox-go
Termbox是一个提供简约API的程序库，允许程序员编写基于文本的用户界面。该库是跨平台的，并且在* nix操作系统上具有基于终端的实现，并且在Windows操作系统上实现了基于winapi控制台的实现。基本思想是以简约的方式抽象出所有主要终端和其他类似终端的API上可用功能的最大公共子集。小型API意味着很容易实现，测试，维护和学习，这就是使termbox成为其独特的库的原因
[nsf/termbox-go](https://github.com/nsf/termbox-go)

---
### olekukonko/tablewriter
终端表格打印
[olekukonko/tablewriter](https://github.com/olekukonko/tablewriter)

---
### pborman/uuid
该项目是从code.google.com/p/go-uuid自动导出的
uuid软件包根据RFC 4122和DCE 1.1 生成并检查UUID ：身份验证和安全服务。
[pborman/uuid](https://github.com/pborman/uuid)

---
### peterh/liner
带有历史的Pure Go line编辑器，受linenoise的启发
[peterh/liner](https://github.com/peterh/liner)


---
### pkg/errors
包错误提供了简单的错误处理原语。
[pkg/errors](https://github.com/pkg/errors)

---
### pmezard/go-difflib
Go-difflib是python 3 difflib软件包的一个部分端口。其主要目标是在纯Go中提供统一的和背景差异化的功能，主要用于测试目的。
[pmezard/go-difflib](https://github.com/pmezard/go-difflib)


---
### prometheus/prometheus
Prometheus是云计算本地计算基金会项目，是一个系统和服务监控系统。它按给定的时间间隔从配置的目标收集指标，评估规则表达式，显示结果，并且如果观察到某些条件为真，则可触发警报。

[prometheus/prometheus](https://github.com/prometheus/prometheus)



---
### rjeczalik/notify
关于类固醇的文件系统事件通知库。
[rjeczalik/notify](https://github.com/rjeczalik/notify)

---
### robertkrimen/otto
软件包otto是Go中本地编写的JavaScript解析器和解释器。
[robertkrimen/otto](https://github.com/robertkrimen/otto)


---
### rs/cors

CORS是一个在Golang中net/http实现Cross Origin Resource Sharing W3规范的处理程序。
[CORS （网络通信技术](https://baike.baidu.com/item/CORS/16411212#viewPageContent)

[rs/cors](https://github.com/rs/cors)



### rs/xhandler
XHandler是net / context和http.Handler。之间的桥梁。

它可以让你net/context在你的处理程序中执行而不牺牲现有的兼容性，http.Handlers也不需要强加特定的路由器。

由于net/context截止日期管理，xhandler能够强制执行每个请求的最后期限，并在客户端意外关闭连接时取消上下文。

你可以创建你自己的net/context感知处理程序，就像你使用http.Handler一样。

在Dailymotion工程博客上阅读更多关于xhandler的信息。
[rs/xhandler](https://github.com/rs/xhandler)

---
### StackExchange/wmi
软件包wmi为Windows WMI提供了一个WQL界面。

注意：它与本地机器上的WMI进行连接，因此它只能在Windows上运行。
[StackExchange/wmi](https://github.com/StackExchange/wmi)


---
### stretchr/testify
Go代码（golang）包提供许多工具来证明您的代码将按照您的意图行事。
[stretchr/testify](https://github.com/stretchr/testify)

---

### syndtr/goleveldb
项目如其名
leveldb的go版本
[syndtr/goleveldb](https://github.com/stretchr/testify)


本著作版权由本博客作者所有 ，如需转载请附上附上出处

