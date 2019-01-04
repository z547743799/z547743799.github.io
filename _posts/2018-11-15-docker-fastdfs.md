---
layout: post
title:  "docker+fastdfs"
date:   2018-11-15
excerpt: "docker-fastdfs"
description: "docker-fastdfs"
tag:
- docker
- fastdfs
comments: true
---

# docker+fastdfs

### 拉取镜像
`docker pull morunchang/fastdfs`

### 查看镜像
`docker images`

### 运行tracker
`docker run -d --name tracker --net=host morunchang/fastdfs sh tracker.sh`

### 运行storage 填写你的ip 和组名
`docker run -d --name storage --net=host -e TRACKER_IP=<your tracker server address>:22122 -e GROUP_NAME=<group name> morunchang/fastdfs sh storage.sh`

### 进入容器
`docker exec -it storage  bash`

### 修改配置文件
`vi data/nginx/conf/nginx.conf`

### 添加

    location /group1/M00 {

    proxy_next_upstream http_502 http_504 error timeout invalid_header;

     proxy_cache http-cache;

     proxy_cache_valid  200 304 12h;

     proxy_cache_key $uri$is_args$args;

     proxy_pass http://fdfs_group1;

     expires 30d;

    }

### 退出

`root:/data/nginx/conf# exit`

### 重启
`docker restart storage`
### 开放短裤22122

`firewall-cmd --zone=public --add-port=22122/tcp --permanent`

### 刷新
`firewall-cmd --reload`

### 然后使用clin端调用就好了

 查看 `ls data/fast_data/data/00/00/`
 文件夹即可知道是否上传成功
 <iframe src="//player.bilibili.com/player.html?aid=25695529&cid=43840093&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>
