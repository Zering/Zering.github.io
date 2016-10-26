---
layout:     post
title:      "CentOS 6.8安装redis-3.2.4"
subtitle:   " \"Redis\""
date:       2016-10-26 10:33:55 
author:     "Zering"
header-img: "img/post-bg-linux.jpg"
catalog: true
tags:
    - Linux
---

### 前期准备

安装redis需要系统已经安装gcc和tcl

gcc安装方法： yum install gcc-c++

tcl安装方法： yum install -y tcl

### 下载redis安装包

进入redis官网：[Redis](http://redis.io/download),查看最新的稳定版本，右键复制下载的链接地址

然后，按以下命令进行操作

#### 创建存放安装包的文件夹

	wget http://download.redis.io/releases/redis-3.2.4.tar.gz

#### 解压安装包

	tar -zxvf redis-3.2.4.tar.gz

#### 进入安装包，进行编译

	cd redis-3.2.4
	make

**注意**
只有事先安装好了gcc和tcl才能编译

### 相关配置

安装好安装包其实就可以运行redis了，但为了跟方便的操作，进行以下配置

#### 将src下的可执行文件redis-server,redis-cli复制到usr下，方便执行
	
	cd src
	cp redis-server /usr/local/bin/
	cp redis-cli /usr/local/bin/

#### 创建配置文件存放的文件夹，方便查看

	mkdir /etc/redis
	mkdir /var/redis
	mkdir /var/redis/log
	mkdir /var/redis/run
	mkdir /var/redis/6379

#### 根据redis根目录下的配置文件模板redis.conf，复制并编写服务器的配置文件

	cd /redis/redis-3.2.4
	cp redis.conf /etc/redis/6379.conf

配置文件以下修改

	vim /etc/redis/6379.conf
	#设置后台运行
	daemonize yes
	#存放线程信息
	pidfile /var/redis/run/redis_6379.pid
	#存放日志信息
	logfile /var/redis/log/redis_6379.log
	#临时文件和备份文件存放地址
	dir /var/redis/6379

#### 运行redis服务

	redis-server /etc/redis/6379.conf

进入redis客户端只需输入`redis-cli`。

