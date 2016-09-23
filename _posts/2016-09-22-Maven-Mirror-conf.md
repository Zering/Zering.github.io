---
layout:     post
title:      "Maven镜像配置"
subtitle:   " \"Maven\""
date:       2016-9-22 15:34:59  
author:     "Zering"
header-img: "img/post-bg-java.jpg"
catalog: true
tags:
    - Tips
---

### Maven镜像配置

#### 前言

由于大中华局域网的影响，Maven在构建时的速度较慢，我们可以通过在设置里面配置国内的镜像来提升构建速度

#### 配置

修改maven根目录下的conf文件夹中的setting.xml文件中的<mirrors></mirrors>

在其中填写国内的镜像库，以下是阿里云镜像库的配置方法：
	
	  <mirrors>
	    <mirror>
	      <id>alimaven</id>
	      <name>aliyun maven</name>
	      <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
	      <mirrorOf>central</mirrorOf>        
	    </mirror>
	  </mirrors>

其他的镜像资源：

	<mirror>  
      <id>CN</id>  
      <name>OSChina Central</name>                                                                                                                         
      <url>http://maven.oschina.net/content/groups/public/</url>  
      <mirrorOf>central</mirrorOf>  
    </mirror> 

	<mirror>    
	      <id>repo2</id>    
	      <mirrorOf>central</mirrorOf>    
	      <name>Human Readable Name for this Mirror.</name>    
	      <url>http://repo2.maven.org/maven2/</url>    
	</mirror>    
	<mirror>    
	      <id>net-cn</id>    
	      <mirrorOf>central</mirrorOf>    
	      <name>Human Readable Name for this Mirror.</name>    
	      <url>http://maven.net.cn/content/groups/public/</url>     
	</mirror>    
	<mirror>    
	      <id>ui</id>    
	      <mirrorOf>central</mirrorOf>    
	      <name>Human Readable Name for this Mirror.</name>    
	     <url>http://uk.maven.org/maven2/</url>    
	</mirror>    
	<mirror>    
	      <id>ibiblio</id>    
	      <mirrorOf>central</mirrorOf>    
	      <name>Human Readable Name for this Mirror.</name>    
	     <url>http://mirrors.ibiblio.org/pub/mirrors/maven2/</url>    
	</mirror>    
	<mirror>    
	      <id>jboss-public-repository-group</id>    
	      <mirrorOf>central</mirrorOf>    
	      <name>JBoss Public Repository Group</name>    
	     <url>http://repository.jboss.org/nexus/content/groups/public</url>    
	</mirror>  
	  
	  
	<mirror>    
	      <id>JBossJBPM</id>   
	　　　　<mirrorOf>central</mirrorOf>   
	　　　　<name>JBossJBPM Repository</name>   
	　　　　<url>https://repository.jboss.org/nexus/content/repositories/releases/</url>  
	</mirror>  
