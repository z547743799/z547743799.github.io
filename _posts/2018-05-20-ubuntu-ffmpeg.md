---
layout: post
title:  "ubuntu使用ffmpeg压缩视频"
date:   2018-05-16
excerpt: "ubuntu使用ffmpeg压缩视频"
description: "ubuntu使用ffmpeg压缩视频"
tag:
- ubuntu
- ffmpeg
- 压缩视频
comments: true
---

# ubuntu使用ffmpeg压缩视频
1、准备工作：
http://ffmpeg.org/download.html 
下载之后，正常安装，然后将bin目录加入全局环境变量；
配置软链
查看是否安装好，以及软件版本：

ffmpeg -version
![](https://img-blog.csdn.net/20180205190848341?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvemhlemhlYmll/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
官方文档： 
http://ffmpeg.org/ffmpeg.html


 
### 一般来说音频影响不大，我们就重点说视频压缩：
压缩用到的参数： 

-i 输入文件的路径或者url； 

-s 设置输出文件的分辨率,wxh； 常见的分辨率有4096*2304,1920*1080,720*576等。

-b:v 输出文件的码率，一般500k左右即可，人眼看不到明显的闪烁，这个是与视频大小最直接相关的；

-strict -2 严格模式

### 例子:
    ffmpeg -i /media/lzw/E/lzw_koyixueyuan/2018-04-28\ 爬虫/视频/001\ -\ 爬虫项目选型.mp4 -b:v 400k -s 1920x1080 -strict -2 /media/lzw/E/lzw_koyixueyuan/2018-04-28\ 爬虫/sds.mp4

 

 
 

参考:
[ffmpeg视频转码压缩](https://blog.csdn.net/zhezhebie/article/details/79263492)
