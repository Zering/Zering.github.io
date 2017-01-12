---
layout:     post
title:      "Java实现多继承的三种方式"
subtitle:   " \"Java Se\""
date:       2017-1-11 13:21:29 
author:     "Zering"
header-img: "img/post-bg-java.jpg"
catalog: true
tags:
    - Tips
---

## Java实现多继承的三种方式

### 前言

Java本身不支持多继承，但我们可以通过变通的方式实现多继承的效果

---

### 正文

### 方式一：接口

虽然Java不能多继承，但是却能实现多个接口，故而可以实现多继承的效果，而且接口也是优先于继承的

示例：

	public class ByInterfaces implements DoA, DoB {

	@Override
	public void a() {
		System.out.println("Do something about A");
	}

	@Override
	public void b() {
		System.out.println("Do something about B");
	}
	
	public void describe() {
		a();
		b();
	}
	
	}
	
	interface DoA {
		void a();
	}
	
	interface DoB {
		void b();
	}

### 方式二：内部类

上述通过接口实现多继承是一种普遍的做法，但是还是要Override所有的方法，事实上我们可以通过内部类的方式完美的实现多继承。

示例：

	public class ByInnerClass {
	
		class FromFather extends Father{
			//可以选择重新strong方法
			@Override
			public void strong() {
				System.out.println("I'm more Strong");
			}
		}
		
		class FromMother extends Mother{
			//也可以不重写，直接使用
		}
		
		public void describe() {
			new FromFather().strong();
			new FromMother().kind();
		}
	}
	
	
	class Father{
		public void strong() {
			System.out.println("I'm Strong");
		}
	}
	
	class Mother{
		public void kind() {
			System.out.println("I'm kind");
		}
	}

### 方式三：接口的默认方法（适用于Java 8）

Java 8的更新为接口提供了默认方法，因此也多了一种实现多继承的方式，实现接口时，可以根据需求决定是否重写默认方法

示例：

	public class ByDefaultMethod implements A,B{
	
		//根据需求决定是否重写a,b方法，两者都可以调用
		public void describe() {
			a();
			b();
		}
		
		@Override
		public void a() {
			System.out.println("I'm override A.a()");
		}
	}
	
	interface A{
		default void a(){
			System.out.println("Do something about A");
		}
	}
	
	interface B{
		default void b(){
			System.out.println("Do something about B");
		}
	}


### 后记

一般来说，接口优先于继承，因为接口更加灵活，易于扩展

[练习代码](https://github.com/Zering/Java-SE-Core/tree/master/src/MultipleInheritance)