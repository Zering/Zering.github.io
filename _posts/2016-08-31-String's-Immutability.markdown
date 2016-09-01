---
layout:     post
title:      "图解java字符串的不变性"
subtitle:   " \"Simple Java String\""
date:       2016-08-31 15:28:28
author:     "Zering"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - Simple Java
---

## 图解java字符串的不变性
英文原文地址[http://www.programcreek.com/2009/02/diagram-to-show-java-strings-immutability/](http://www.programcreek.com/2009/02/diagram-to-show-java-strings-immutability/)

这里通过一组图片来说明java String的不变性

### 1.声明一个string
`String s = "abcd;"`<br/>
s存储了一个string对象的引用，下方的箭头应该理解为“内存的引用”
![](http://www.programcreek.com/wp-content/uploads/2009/02/String-Immutability-1.jpeg)

### 2.将一个string变量赋值给另一个string变量
`String s2 = s;`<br/>
由于它们是相同的string对象,s2保存相同的引用
![](http://www.programcreek.com/wp-content/uploads/2009/02/String-Immutability-2.jpeg)

### 3.合并字符串
`s = s.concat("ef")`<br/>
S现在保存的引用是一个新建的string对象
![](http://www.programcreek.com/wp-content/uploads/2009/02/string-immutability-650x279.jpeg)

### 总结
字符串一旦被在内存（堆）中创建，它就是不可变的了。注意String的任何方法都不能改变字符串本身，而是返回一个新的字符串。

如果我们需要一个可变的字符串，可以适应StringBuffer或者StringBuilder。否则，垃圾回收会因为每次新建一个字符串而耗费大量的时间。

下篇：讲解StringBuffer的惯用法