---
layout: post
title:  "docker+Elasticsearch"
date:   2018-11-15
excerpt: "docker-Elasticsearch"
description: "docker-Elasticsearch"
tag:
- docker
- elasticsearch
comments: true
---

# docker+Elasticsearch

### 下载Elasticsearch

`docker pull docker.elastic.co/elasticsearch/elasticsearch:6.5.0`
### 运行Elasticsearch

`docker run -d --name es -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" docker.elastic.co/elasticsearch/elasticsearch:6.5.0`
### 进入Elasticsearch
`docker exec -it es bash`

### 显示文件
`ls`
### 结果如下：
`LICENSE.txt  README.textile  config  lib   modules
NOTICE.txt   bin             data    logs  plugins`

### 进入配置文件夹
`cd config`

### 显示文件
`ls`
 ### 结果如下：
`elasticsearch.keystore  ingest-geoip  log4j2.properties  roles.yml  users_roles
elasticsearch.yml       jvm.options   role_mapping.yml   users`

###  修改配置文件
`vi elasticsearch.yml`

###  加入跨域配置
`http.cors.enabled: true`

`http.cors.allow-origin: "*"`
### 重启容器
`docker restart es`
###  拉取镜像
`docker pull mobz/elasticsearch-head:5`
### 运行容器
`docker run -d --name es_admin -p 9100:9100 mobz/elasticsearch-head:5`
### 展示图
![展示图](https://images2018.cnblogs.com/blog/534030/201808/534030-20180802225531963-1621975.png)

[全文摘自博客](https://www.cnblogs.com/jianxuanbing/p/9410800.html)
`