---
layout:     post
title:      "ElasticSearch入门-后续"
subtitle:   " \"ElasticSearch的基础操作\""
date:       2017-09-25 17:29:00
author:     "Zering"
header-img: "img/post-bg-java.jpg"
catalog: true
tags:
    - Java
---

### 前言

本来想把本篇和上一篇合一起的，太久没有写长文，还是先从短博文开始适应一下，下面主要介绍了elasticsearch原生的一些简单操作，如索引，CRUD等等。

### 索引创建

##### 方式一：直接head插件网页操作

直接在 *索引* 项新建索引

![](/img/blog/elasticsearch/head-new-index.png)

在复合查询项，可以编写索引内的数据结构（mappings），该方式了解即可



##### 方式二：通过elasticsearch提供的api接口创建

![](/img/blog/elasticsearch/api-new-index.png)



使用PUT方式，master访问地址直接加索引名，body里面用json格式编写需要的信息

setting内是分片数(number_of_shards)、副本数(number_of_replicas)

mappings内是数据结构信息

```json
{
	"settings":{
		"number_of_shards" : 3,
		"number_of_replicas" : 1
	},
	"mappings" : {
		"man" : {
			"properties" : {
				"name" : {
					"type" : "text"
				},
				"country" : {
					"type" : "keyword"
				},
				"age" : {
					"type" : "integer"
				},
				"date" : {
					"type" : "date",
					"format" : "yyyy-MM-dd HH:mm:ss||yyyy-MM-dd||epoch_millis"
				}
			}
		},
		"woman" : {}
	}
}
```



使用postman的好处在于，能够纠错json格式。



### 插入

分为指定文档id插入和自动文档id插入。

直接在url中指定id即可指定id插入，如下图：

![](/img/blog/elasticsearch/insert_appoint_id.png)



自动生成文档id即不写文档id，elasticsearch会自动生成一个string类型的id，并返回。

### 修改

修改文档时需要指定文档的id，而且修改的url为_update

可以通过文档的方式修改，也可以通过脚本的方式来修改数据信息，如下图：

文档方式修改（关键字：doc）

![](/img/blog/elasticsearch/update-doc.png)

脚本方式修改（关键字：script）

![](/img/blog/elasticsearch/update-script.png)

### 删除和查询

删除操作很简单，直接用delete方法，如果指定到id即删除指定的文档，如果用delete方法，url只写到索引，则直接删除整个索引。

查询在url要使用_search，使用post方法，在json中编写查询条件，主要关键词 “query”、“sort”，聚合查询关键词“aggs"



### 在Java中使用elasticsearch的基本操作

根据elasticsearch提供的api，我们可以编写java程序来对es数据进行操作，具体[demo实例](https://github.com/Zering/elasticsearch)



### 后记

可能还是不太习惯写博文，后续改变一些策略，多写代码，厚积薄发



代码练习清单：

完成junit3.8简单版本

完成junit4.0主要是加入注解的方式

完成一个简单的MVC框架

整理重构一下博客主题

整理一个实用的SpringBoot快速开发框架（后续具体化）

对mina和netty进行二次包装