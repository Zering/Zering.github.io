---
layout:     post
title:      "ElasticSearch入门-上篇"
subtitle:   " \"ElasticSearch的安装\""
date:       2017-09-24 17:29:00
author:     "Zering"
header-img: "img/post-bg-java.jpg"
catalog: true
tags:
    - Java
---

## ElasticSearch入门

### 前言

在观看学习了慕课网的视频 [ElasticSearch入门](http://www.imooc.com/learn/889) 之后，在本机上完成了所有教程的复现，写下笔记，方便日后复习。 练习源码 [elasticsearch](https://github.com/Zering/elasticsearch) 。

注意：该环境需要JDK，node.js的环境，我的机器jdk版本为1.8.0_111,node.js版本为v.8.1.4

### 安装ElasticSearch服务

直接从官网下载最新版本的[elasticsearch-5.6.0.tar.gz](https://www.elastic.co/products/elasticsearch) 解压安装后，直接运行bin目录下的elasticsearch即可开启一个单实例的elasticsearch的服务，主要命令如下：

```sh
tar -zxvf elasticsearch-5.6.0.tar.gz
sh ./elasticsearch-5.6.0/bin/elasticsearch
```

默认http访问端口：9200 （用于网页查看），TCP访问端口：9300 （用于程序数据交互）

服务启动后，可通过localhost:9200查看服务器信息， 如下所示：

![](/img/blog/elasticsearch/elasticsearch-origin.png)

### 多节点分布式安装

elasticsearch可以通过修改配置的方式方便的进行多节点的部署，在多个节点中，它们的cluster.name必须相同（默认是elasticsearch），必须有一个master节点，slave节点要设置寻找master的地址。

配置文件为elasticsearch-5.6.0/config文件夹下的elasticsearch.yml文件

master配置：

```shell
cluster.name: wali #可以自定义，但多个节点的cluster.name必须保持一致
node.name: master
node.master: true
network.host: 127.0.0.1
```

slave节点，直接cp之前下载的tar包解压出来即可，需要多个slave可以随意复制，但要保证使用的端口不同

***ps: 如果你直接cp了master的文件夹，记得删除掉data文件夹***

slave配置

```shell
cluster.name: wali
node.name: slave1
network.host: 127.0.0.1

http.port: 8200 #默认的9200已经被master占用了，可以随便自定义一个可用的端口
discovery.zen.ping.unicast.hosts: ["127.0.0.1"] #用于寻找master
```

### 安装插件

原生的服务器信息界面（json格式）对用户并不友好，我们可以通过安装一个head插件，来提升界面友好度。

*PS:该插件需要node.js的支持*

直接从github上下载源码[elasticsearch-head](https://github.com/mobz/elasticsearch-head) 

下载后直接用npm安装启动即可，命令如下：

```sh
cd elasticsearch-head
npm install
npm run start
```

由于插件与elasticsearch之间的访问有跨域问题，所有需要修改elasticsearch的配置，修改elasticseach-5.6.0/config文件夹下的elasticsearch.yml文件，添加以下信息：

```shell
http.cors.enabled: true
http.cors.allow-origin: "*"
```

访问localhost:9100即可查看服务器信息，包括 节点，索引，数据等信息，还可以直接对数据进行操作

![](/img/blog/elasticsearch/elasticsearch-head.png)

### 总结

至此，我们就完成了elasticsearch服务器的安装和运行。

下篇介绍elasticsearch的基础操作。



#### 后记

这是设定计划的第一周，然而效果并不是很理想，但是也有所改善

周一、周二完成了elasticsearch的视频学习和复现，基本完成任务，周三周四加班太晚，回家已经10点半了而且有点累，读书的任务没有完成。周六加班，周日太累直接睡到了中午才醒，在5点多钟开始写博文，墨迹到了8点才完成。另外，博文在写的太长的时候有些疲劳，所以分成了三部分（周三前全部完成）来写，等以后习惯了再尽量在一篇博文中输出更多的信息。

下周计划，系统学习一下spring-boot的知识，先看一波视频教程，再对照官方api学习重点知识。



---

祝好，加油！！！