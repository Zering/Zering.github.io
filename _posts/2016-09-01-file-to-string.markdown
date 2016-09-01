---
layout:     post
title:      "将一个文件转换成String"
subtitle:   " \"Simple Java String\""
date:       2016-09-01 08:56:28
author:     "Zering"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - Simple Java
---
## 将一个文件转换成String

英文原文地址：[http://www.programcreek.com/2011/11/java-convert-a-file-into-a-string/](http://www.programcreek.com/2011/11/java-convert-a-file-into-a-string/)

怎么将一个文件的内容转换成字符串呢？

下面这段java代码将告诉你如何做。实际应用时，filePath要改成文件地址
<!--lang: java--> 
	public static String readFileToString() throws IOException {
			File dirs = new File(".");
			String filePath = dirs.getCanonicalPath() + File.separator+"src"+File.separator+"TestRead.java";
	 		
			//构建一个StringBuilder并指定初始化大小
			StringBuilder fileData = new StringBuilder(1000);
			BufferedReader reader = new BufferedReader(new FileReader(filePath));
	 
			char[] buf = new char[1024];
			int numRead = 0;
			while ((numRead = reader.read(buf)) != -1) {
				String readData = String.valueOf(buf, 0, numRead);
				fileData.append(readData);
				buf = new char[1024];
			}
	 
			reader.close();
	 
			String returnStr = fileData.toString();
			System.out.println(returnStr);
			return returnStr;
		}

说是String对象是完全正确，它实际上是一个StringBuffer。StringBuffer和StringBuilder类似，他们都是为了可变的字符串，但StringBuffer是线程安全的。这里使用StringBuilder（非同步）是因为它比StringBuffer更快。

从程序使用的角度来看，String，StringBuffer，StringBuilder都是字符串。