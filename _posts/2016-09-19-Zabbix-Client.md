---
layout:     post
title:      "zabbix客户端安装"
subtitle:   " \"zabbix\""
date:       2016-9-19 15:26:51 
author:     "Zering"
header-img: "img/post-bg-linux.jpg"
catalog: true
tags:
    - Linux
---

### Zabbix客户端安装

#### Window

##### 1、  下载与解压

地址: [http://www.zabbix.com/downloads/2.2.0/zabbix_agents_2.2.0.win.zip](http://www.zabbix.com/downloads/2.2.0/zabbix_agents_2.2.0.win.zip)

解压zabbix_agents_2.2.0.win.zip

conf目录存放是agent配置文件 bin文件存放windows下32位和64位安装程序

##### 2、  配置与安装

	1. 配置zabbix agent相关配置。
	   找到conf下的配置文件 zabbix_agentd.win.conf ，修改LogFile、Server、Hostname这三个参数。具体配置如下：
		LogFile=E:\zabbix\zabbix_agentd.log
		Server=192.168.3.64  #zabbix server地址
		Hostname=machine01 
		ServerActive=192.168.3.64  #zabbix server地址
		其中logfile是zabbix日志存放地址。Server 是zabbix服务端ip地址。Hostname是本机机器名。（与服务器端添加的主机名一致）
	2. 安装agent
		在windows控制台下执行以下命令：
		(以文件解压在E:\zabbix下为例)
		E:\zabbix\bin\win32\zabbix_agentd.exe -c E:\zabbix\conf\zabbix_agentd.win.conf –i 
	3. 启动agent客户端
		启动命令如下：
		E:\zabbix\bin\win32\zabbix_agentd.exe -c E:\zabbix\conf\zabbix_agentd.win.conf –s

###### 3、设置开机自启动

运行services.msc,进入服务列表，设置Zabbix Agent服务为开机自启动

###### 4、防火墙

打开10050端口，添加入站规则

#### CentOS

##### 1、下载安装包

wget http://jaist.dl.sourceforge.net/project/zabbix/ZABBIX%20Latest%20Stable/2.2.2/zabbix-2.2.2.tar.gz

##### 2、安装客户端

	[root@ zabbix-2.2.2]#groupadd zabbix
	[root@ zabbix-2.2.2]#useradd -g zabbix zabbix
	[root@ zabbix-2.2.2]#tar zxvf zabbix-2.2.2.tar.gz
	[root@ zabbix-2.2.2]#cd zabbix-2.2.2
	[root@ zabbix-2.2.2]#./configure --prefix=/usr/local/zabbix_agent --enable-agent
	[root@ zabbix-2.2.2]#make install

##### 3、修改配置文件(zabbix-2.2.2目录）

	a) cp misc/init.d/fedora/core/zabbix_agentd /etc/init.d/
	b) sed -i 's/BASEDIR=\/usr\/local/BASEDIR=\/usr\/local\/zabbix_agent/g' /etc/init.d/zabbix_agentd

##### 4、添加服务端口（如果没有默认添加端口，按以下操作手动添加）

	[root@ zabbix-2.2.2]# cat >>/etc/services<<EOF
	> zabbix-agent 10050/tcp #zabbix agent
	> zabbix-agent 10050/udp #zabbix agent
	> zabbix-trapper 10051/tcp #zabbix trapper
	> zabbix-trapper 10051/udp #zabbix trapper
	> EOF

##### 5、配置客户端

	1.	vi /usr/local/zabbix_agent/etc/zabbix_agentd.conf
	2.	LogFile=/usr/local/zabbix_agent/zabbix_agentd.log
	3.	Server=192.168.3.64  #zabbix server地址
	4.	Hostname=machine02  #主机名，与服务器设置一致
	5.	ServerActive=192.168.3.64  #zabbix server地址

##### 6、启动客户端
	启用服务
	a) /etc/init.d/zabbix_agentd start
	b)  echo "/etc/init.d/zabbix_agentd start" >> /etc/rc.local	（开机执行命令）

##### 7、防火墙策略
	1.	iptables -I INPUT -p tcp --dport 10050 -j ACCEPT


#### 安装完成后
在服务器上配置对应的主机和模板

组态-->主机-->创建主机-->
	
	主机名称：写客户端对应的Hostname
	IP地址：填客户端的IP地址

