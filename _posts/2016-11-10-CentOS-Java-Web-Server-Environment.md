---
layout:     post
title:      "CentOS 6.8 Java Web应用服务器环境搭建"
subtitle:   " \"jdk tomcat mysql\""
date:       2016-11-10 9:56:06 
author:     "Zering"
header-img: "img/post-bg-linux.jpg"
catalog: true
tags:
    - Linux
---

### CentOS 6.8 Java Web应用服务器环境搭建

#### 安装说明

CentOS 6.8系统使用的是网易的镜像，可访问[网易镜像站](http://mirrors.163.com/)进行下载；

更换网易的[yum源](http://mirrors.163.com/.help/centos.html)，提升下载速度

#### 软件安装版本

**（openJDK）JDK:** 1.7

**tomcat:**	7.0

**mysql:** 5.1.73

说明： 这里介绍的是较为通用的安装方式，所以版本可以根据自己的需求进行选择

#### JDK安装

##### 1. 查看老版本的JDK
	rpm -qa | grep jdk
##### 2. 卸载所有查询出来的结果
	rpm -e -nodeps 查询出来的结果
##### 3. 查看yum仓库中的jdk
	yum search jdk
##### 4. 安装jdk相关软件
	yum -y install java-1.7.0-openjdk*
**注意：**可以根据需求选择1.6.0、1.7.0、1.8.0；结尾有一个<B color='red'>*</B>号的通配符
##### 5.设置环境变量
	#set java environment
	JAVA_HOME=/usr/lib/jvm/java-1.7.0-openjdk-1.7.0.111.x86_64
	JRE_HOME=$JAVA_HOME/jre
	CLASS_PATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib
	PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin
	export JAVA_HOME JRE_HOME CLASS_PATH PATH
##### 6.验证
	输入java -version 显示如下内容
	java version "1.7.0_111"
	OpenJDK Runtime Environment (rhel-2.6.7.2.el6_8-x86_64 u111-b01)
	OpenJDK 64-Bit Server VM (build 24.111-b01, mixed mode)

	或者 输入javac 显示如下内容
	Usage: javac <options> <source files>
	where possible options include:
	  -g                         Generate all debugging info
	  -g:none                    Generate no debugging info
	  -g:{lines,vars,source}     Generate only some debugging info
	  -nowarn                    Generate no warnings
	  -verbose                   Output messages about what the compiler is doing
	  -deprecation               Output source locations where deprecated APIs are used
	  -classpath <path>          Specify where to find user class files and annotation processors
	  -cp <path>                 Specify where to find user class files and annotation processors
	  -sourcepath <path>         Specify where to find input source files
	  -bootclasspath <path>      Override location of bootstrap class files
	  -extdirs <dirs>            Override location of installed extensions
	  -endorseddirs <dirs>       Override location of endorsed standards path
	  -proc:{none,only}          Control whether annotation processing and/or compilation is done.
	  -processor <class1>[,<class2>,<class3>...] Names of the annotation processors to run; bypasses default discovery process
	  -processorpath <path>      Specify where to find annotation processors
	  -d <directory>             Specify where to place generated class files
	  -s <directory>             Specify where to place generated source files
	  -implicit:{none,class}     Specify whether or not to generate class files for implicitly referenced files
	  -encoding <encoding>       Specify character encoding used by source files
	  -source <release>          Provide source compatibility with specified release
	  -target <release>          Generate class files for specific VM version
	  -version                   Version information
	  -help                      Print a synopsis of standard options
	  -Akey[=value]              Options to pass to annotation processors
	  -X                         Print a synopsis of nonstandard options
	  -J<flag>                   Pass <flag> directly to the runtime system
	  -Werror                    Terminate compilation if warnings occur
	  @<filename>                Read options and filenames from file

#### Tomcat安装

##### 1.下载
	
通过[apache官网](http://tomcat.apache.org/)获取需要的Tomcat版本的下载地址
右键点击Core下的tar.gz文件，选择 复制链接地址

	1. 创建一个存放Tomcat的目录
	mkdir /mydata
	cd /mydata
	2. 根据复制的地址下载安装包
	wget http://mirrors.hust.edu.cn/apache/tomcat/tomcat-7/v7.0.72/bin/apache-tomcat-7.0.72.tar.gz

##### 2.解压安装
	
	tar -zxvf apache-tomcat-7.0.72.tar.gz
##### 3.别名设置

	设置别名，方便启动和关闭	
	alias starttomcat='cd /mydata/apache-tomcat-7.0.72/bin;./catalina.sh start'
	alias stoptomcat='cd /mydata/apache-tomcat-7.0.72/bin;./catalina.sh stop'

#### Mysql安装

##### 1.检查卸载旧版本

	1. 查询所有已安装旧版本
	rpm -qa | grep mysql
	2. 卸载查询到的所有相关旧版本
	rpm -e --nodeps mysql
	rpm -e --nodeps mysql-libs
	etc..

##### 2.直接yum安装mysql
	yum -y install mysql mysql-server mysql-devel

##### 3.启动服务，设置开机自启动
	service mysqld start
	chkconfig mysqld on

##### 4.设置用户和密码
	mysqladmin -u root password '你的密码'

##### 5.设置远程访问数据库的权限
	
	1.登录数据库
	mysql -u root -p
	（输入你的密码登录）
	2.添加权限
	GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '你的密码' WITH GRANT OPTION;
	3.刷新权限
	flush privileges;

#### 防火墙问题

方式一：添加防火墙策略
	
	vim etc/sysconfig/iptables
	添加以下信息
	iptables -A INPUT -p tcp --dport 808080 -j ACCEPT
	iptables -A INPUT -p tcp --dport 3306 -j ACCEPT
	
方式二：直接关闭防火墙
	
	service iptables stop
	chkconfig iptables off
