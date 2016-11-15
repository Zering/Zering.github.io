---
layout:     post
title:      "Spring装配Bean的三种方式"
subtitle:   " \"Java\""
date:       2016-11-15 15:27:34 
author:     "Zering"
header-img: "img/post-bg-java.jpg"
catalog: true
tags:
    - Java
---

# Spring装配Bean的三种方式

### 隐式的bean发现机制和自动装配

要实现**隐式的bean发现机制和自动装配**需要完成两个方面的配置

1. 组件扫描（component scanning） :Spring会自动发现应用上下文中所创建的Bean；
2. 自动装配（autowiring）:Spring自动满足bean之间的依赖。

#### 创建可被发现的bean

要实现组件的扫描，需要在组件类上加上@Component及其衍生的注解（如@Controller等），同时需要开启应用上下文的包扫描的功能，开启包扫描有两种方式，可以在java配置类上加上@ComponentScan的注解，也可以在xml配置文件中使用<context:component-scan>元素，它们都可以设定 base-package来设置需要扫描的包。

#### 为组件扫描的bean命名

Spring应用上下文对于扫描到的没有明确指明id的bean,会将其id设置为首字母小写的类名。

通过注解有两种方式可以给bean命名，@Component("beanName")或者@Named("beanName")（不提倡）

#### 通过为bean添加注解实现自动装配

我们可以在需要自动装配的bean的构造器上使用@Autowired来实现自动装配，当然也能子啊Setter方法上使用@Autowired;不管是构造器、Setter方法还是其他的方法，Spring都会尝试满足方法参数上所声明的依赖。假如有且只有一个bean匹配依赖需求的话，那么，这个bean将会被装配进来。

可以用@Inject、@Resource来代替@Autowired的可以根据自己的喜好来

### 通过Java代码装配bean

在Java配置类中，我们可以过@Bean的方式来创建bean,通过调用方法的方式来实现bean的装配，例：
	@Configuration
	public class Javaconfig{
		//创建一个普通bean
		@Bean
		public CompactDisc sgtPeppers(){
			return new SgtPeppers();
		}
		//创建注入一个bean的bean
		@Bean
		public CDPlayer cdPlayer(){
			return new CDPlayer(sgtPeppers);
		}

		//另一种注入Bean更简单的方式
		@Bean
		public CDPlayer cdPlayer(CompactDisc compactDisc){
			return new CDPlayer(compactDisc);
		}
	}

### 通过XML装配bean

通过XML来装配bean时，我们需要一些xsd配置，以下给出一个简单的示例

	<?xml version="1.0" encoding="UTF-8"?>
	<beans xmlns="http://www.springframework.org/schema/beans"
		xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		xsi:schemaLocation="http://www.springframework.org/schema/beans    
	                        http://www.springframework.org/schema/beans/spring-beans.xsd    
	                        http://www.springframework.org/schema/context">
		<!--创建一个简单bean,id将会默认为com.app.soundsystem.SgtPeppers#0
		<bean class="com.app.soundsystem.SgtPeppers" />-->
		<!--对于需要引用的bean,我们一般会通过id来手动指定-->
		<bean id="compactDisc" class="com.app.soundsystem.SgtPeppers" />
	</beans>

有时我们需要在bean初始化时给它注入初始化的bean,我们可以使用<constructor-arg>元素，示例：
	
	<bean id="cdPlayer" class="com.app.soundsystem.CDPlayer">
		<constructor-arg ref="compactDisc">
	</bean>

在Spring3.0后有一种c命名空间的替代方案，需要在头文件中引入c命名空间，注入方式如下：
	<bean id="cdPlayer" class="com.app.soundsystem.CDPlayer">
		<c:cd-ref="compactDisc">
属性名以“c:”开头，也就是命名空间的前缀。接下来就是要装配的构造器参数名，在此之后是“-ref”，这是一个命名的约定，它会告诉Spring，正在装配的是一个bean的引用，这个bean的名字是compactDisc。

如果我们需要将某个值直接注入到参数中，可以使用如下方式：

	<bean id="blankDisc" class="com.app.soundsystem.BlankDisc">
		<constructor-arg value="Lonely Hearts Club">
		<constructor-arg value="The Beatles">
	</bean>

c命名空间的方式（这里只写一种，其他的可以查看官网资料）

	<bean id="blankDisc" class="com.app.soundsystem.BlankDisc"
		c:_0="Lonely Hearts Club"
		c:_1="The Beatles"/>

### 混合配置
#### 在JavaConfig中引用XML配置

1. 通过@Import能导入其他的Java配置类，如在配置类上写@Import({OneConfig.class,AnotherConfig.class})
2. 通过@ImportResource能导入XML配置文件，如@ImportResource({"classpath:config1.xml","classpath:config2.xml"})

#### 在xml配置中引用JavaConfig

1. 引入JavaConfig，直接把配置类当做一个bean文件，如 `<bean class="com.app.soundsystem.Config"/>`
2. 引入其他的XML文件，如`<import resource="config2.xml">`