---
layout:     post
title:      "记录Github上不错的工具项目（Java相关）"
subtitle:   " \"Open Source\""
date:       2016-12-14 16:36:15 
author:     "Zering"
header-img: "img/post-bg-java.jpg"
catalog: true
tags:
    - Tips
---

> “Github,程序员交(gao)友(ji)平台”


## 前言

排名无无先后顺序，想到哪个写哪个

---

## 正文

### 1.JSON工具：fastJson

> Fastjson is a Java library that can be used to convert Java Objects into their JSON representation. It can also be used to convert a JSON string to an equivalent Java object. Fastjson can work with arbitrary Java objects including pre-existing objects that you do not have source-code of.

alibab的一个对象/JSON互转工具

项目地址：<a href="https://github.com/alibaba/fastjson" target="_blank">fastJson</a>

---

### 2.日志工具：logback

依赖于slf4j的一个日志lib，比log4j更快，功能更丰富，也更好使用

logback的优势可见：<a href="http://logback.qos.ch/reasonsToSwitch.html" target="_blank">从log4j迁移到logback的理由</a>

项目地址：<a href="https://github.com/qos-ch/logback" target="_blank">logback</a>

---

### 3.汉字转拼音：JPinyin

>JPinyin是一个汉字转拼音的Java开源类库，在PinYin4j的功能基础上做了一些改进。

>【JPinyin主要特性】

>1、准确、完善的字库；

>Unicode编码从4E00-9FA5范围及3007（〇）的20903个汉字中，JPinyin能转换除46个异体字（异体字不存在标准拼音）之外的所有汉字；

>2、拼音转换速度快；

>经测试，转换Unicode编码从4E00-9FA5范围的20902个汉字，JPinyin耗时约100毫秒。

>3、多拼音格式输出支持；

>JPinyin支持多种拼音输出格式：带音标、不带音标、数字表示音标以及拼音首字母输出格式；

>4、常见多音字识别；

>JPinyin支持常见多音字的识别，其中包括词组、成语、地名等；

>5、简繁体中文转换；

>6、支持添加用户自定义字典；

一个很好用的汉字转拼音项目，拯救了看着一千多个名字茫然的我...不过名字里面的多音字有点问题，比如把“曾”姓装换成了“ceng”

项目地址：<a href="https://github.com/stuxuhai/jpinyin" target="_blank">jpinyin</a>

---

### 4.序列化工具：protostuff

一个序列化/反序列化工具，速度比JDK自带的序列化方式快很多

项目地址：<a href="https://github.com/protostuff/protostuff" target="_blank">protostuff</a>

---

### 5.字符串工具：strman

对字符串进行各种操作的一个工具

项目地址：<a href="https://github.com/shekhargulati/strman-java" target="_blank">strman</a>

---
**未完待续**

## 后记

<a href="https://github.com/Zering?tab=stars" target="_blank">我在github上的一些收藏</a>
