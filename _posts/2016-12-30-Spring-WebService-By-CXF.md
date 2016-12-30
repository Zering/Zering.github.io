---
layout:     post
title:      "Spring整合CXF实现WebService"
subtitle:   " \"Java\""
date:       2016-12-28 13:46:14 
author:     "Zering"
header-img: "img/post-bg-java.jpg"
catalog: true
tags:
    - Java
---

# Spring整合CXF实现WebService

### WebService简介及原理

WebService是什么呢？顾名思义，是Web系统提供的服务，其目的呢，往大了说：是系统实现多异构模块协同合作，服务实现SOA（Services oriented Architecture面向服务的体系结构），往小了说：我们的系统模块能够定义一些功能开放给其他模块，也可以调用其他模块的一些功能。

WebService的实现基本原理就是选择一个各异构系统（结构、开发方式、运行环境等都不完全相同）都能处理的交互信息的方式，让各系统模块能够“听懂”彼此的请求，也可以“告诉”彼此自己的请求，进而实现功能的调用，将追求的功能转为一组合适的通用交互API，开发者只需要根据开放出的API在自己的系统中发出合乎要求的请求就可以了。因此，XML这一相对来说通用性非常强的消息传递方式被选为了完成这一任务的“传递语言”：服务发布方用XML来描述自己系统内开放功能的XML格式的API，其中方法的名称、入参、返回值等都在XML中有明确规定，服务调用方则将自己需要功能的方法、入参、返回值等信息通过XML告知服务发布方，如此一来，就实现了WebService的交互。

### 主流的WebService框架

1、JWS（Java WebService）是Java语言对WebService服务的一种实现，用来开发和发布服务。而从服务本身的角度来看JWS服务是没有语言界限的。但是Java语言为Java开发者提供便捷发布和调用WebService服务的一种途径。这是最原生的JAVA语言层面上支持的WebService，只不过功能较弱。

2、Axis2是Apache下的一个重量级WebService框架，准确说它是一个Web Services / SOAP / WSDL 的引擎，是WebService框架的集大成者，它能不但能制作和发布WebService，而且可以生成Java和其他语言版WebService客户端和服务端代码。这是它的优势所在。但是，这也不可避免的导致了Axis2的复杂性，使用过的开发者都知道，它所依赖的包数量和大小都是很惊人的，打包部署发布都比较麻烦，不能很好的与现有应用整合为一体。但是如果你要开发Java之外别的语言客户端，Axis2提供的丰富工具将是你不二的选择。

3、XFire是一个高性能的WebService框架，在Java6之前，它的知名度甚至超过了Apache的Axis2，XFire的优点是开发方便，与现有的Web整合很好，可以融为一体，并且开发也很方便。但是对Java之外的语言，没有提供相关的代码工具。XFire后来被Apache收购了，原因是它太优秀了，收购后，随着Java6 JWS的兴起，开源的WebService引擎已经不再被看好，渐渐的都败落了。

4、CXF是Apache旗下一个重磅的SOA简易框架，它实现了ESB（企业服务总线）。CXF来自于XFire项目，经过改造后形成的，就像目前的Struts2来自WebWork一样。可以看出XFire的命运会和WebWork的命运一样，最终会淡出人们的视线。CXF不但是一个优秀的Web Services / SOAP / WSDL 引擎，也是一个不错的ESB总线，为SOA的实施提供了一种选择方案，当然他不是最好的，它仅仅实现了SOA架构的一部分。

### Spring集成CXF

#### 普通方式
	
1.引入jar包

maven方式

	<dependency>
	    <groupId>org.apache.cxf</groupId>
	    <artifactId>apache-cxf</artifactId>
	    <version>3.0.10</version>
	</dependency>

或者可以到apache的官网去下载

2.编写webservice类的接口

	package com.app.service;
	
	import javax.jws.WebService;
	import javax.jws.soap.SOAPBinding;
	import javax.jws.soap.SOAPBinding.Style;
	
	@WebService  
	@SOAPBinding(style = Style.RPC) 
	public interface IWebService {
	
		public String sayHello(String string);
	}

3.编写实现类
	package com.app.service.impl;
	
	import javax.jws.WebService;
	import javax.jws.soap.SOAPBinding;
	import javax.jws.soap.SOAPBinding.Style;
	
	import com.app.service.IWebService;
	
	@WebService(endpointInterface="com.app.service.IWebService",serviceName="webservice1")
	@SOAPBinding(style = Style.RPC)
	public class WebserviceImpl implements IWebService {
	
		@Override
		public String sayHello(String string) {
			return "hello "+string;
		}
	}


4.配置application-cxf.xml

配置需要装配的bean

	<?xml version="1.0" encoding="UTF-8"?>
	<beans xmlns="http://www.springframework.org/schema/beans"
		xmlns:context="http://www.springframework.org/schema/context"
		xmlns:jaxws="http://cxf.apache.org/jaxws" xmlns:jaxrs="http://cxf.apache.org/jaxrs"
		xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		xsi:schemaLocation="http://www.springframework.org/schema/beans 
	    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
	    http://www.springframework.org/schema/context
	    http://www.springframework.org/schema/context/spring-context-3.0.xsd
	    http://cxf.apache.org/jaxws 
	    http://cxf.apache.org/schemas/jaxws.xsd
	    http://cxf.apache.org/jaxrs
	    http://cxf.apache.org/schemas/jaxrs.xsd">
	
		<import resource="classpath*:META-INF/cxf/cxf.xml" />
		<import resource="classpath*:META-INF/cxf/cxf-extension-soap.xml" />
		<import resource="classpath*:META-INF/cxf/cxf-servlet.xml" />
	
		<bean id="inMessageInterceptor" class="org.apache.cxf.interceptor.LoggingInInterceptor" />
		<bean id="outLoggingInterceptor" class="org.apache.cxf.interceptor.LoggingOutInterceptor" />
	
		<!--普通方式-->
		<jaxws:endpoint implementor="com.app.service.impl.WebserviceImpl"
			address="/webservice1"></jaxws:endpoint>
	</beans>

5.配置web.xml

设置webservice的访问路径

	<!--加上application-cxf.xml的加载-->
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>classpath:spring-mybatis.xml,classpath:application-cxf.xml,classpath:redis-context.xml</param-value>
	</context-param>

	<!-- For WebService -->
	<servlet>
		<servlet-name>CXFServlet</servlet-name>
		<servlet-class>org.apache.cxf.transport.servlet.CXFServlet</servlet-class>
		<load-on-startup>2</load-on-startup>
	</servlet>
	<servlet-mapping>
		<servlet-name>CXFServlet</servlet-name>
		<url-pattern>/WebService/*</url-pattern>
	</servlet-mapping>

启动服务器后，访问http://localhost:8080/MyWeb/WebService/webservice1/sample?wsdl可以获取一下信息：

	<wsdl:definitions xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:wsdl="http://schemas.xmlsoap.org/wsdl/" xmlns:tns="http://impl.service.app.com/" xmlns:soap="http://schemas.xmlsoap.org/wsdl/soap/" xmlns:ns2="http://schemas.xmlsoap.org/soap/http" xmlns:ns1="http://service.app.com/" name="webservice1" targetNamespace="http://impl.service.app.com/">
	<wsdl:import location="http://localhost:8080/MyWeb/WebService/webservice1/sample?wsdl=IWebService.wsdl" namespace="http://service.app.com/"></wsdl:import>
	<wsdl:binding name="webservice1SoapBinding" type="ns1:IWebService">
	<soap:binding style="rpc" transport="http://schemas.xmlsoap.org/soap/http"/>
	<wsdl:operation name="sayHello">
	<soap:operation soapAction="" style="rpc"/>
	<wsdl:input name="sayHello">
	<soap:body namespace="http://service.app.com/" use="literal"/>
	</wsdl:input>
	<wsdl:output name="sayHelloResponse">
	<soap:body namespace="http://service.app.com/" use="literal"/>
	</wsdl:output>
	</wsdl:operation>
	</wsdl:binding>
	<wsdl:service name="webservice1">
	<wsdl:port binding="tns:webservice1SoapBinding" name="WebserviceImplPort">
	<soap:address location="http://localhost:8080/MyWeb/WebService/webservice1/sample"/>
	</wsdl:port>
	</wsdl:service>
	</wsdl:definitions>

#### rest风格

1.编写rest风格接口

	package com.app.service;
	
	import java.io.IOException;
	
	import javax.servlet.http.HttpServletRequest;
	import javax.servlet.http.HttpServletResponse;
	import javax.ws.rs.Consumes;
	import javax.ws.rs.DELETE;
	import javax.ws.rs.GET;
	import javax.ws.rs.POST;
	import javax.ws.rs.PUT;
	import javax.ws.rs.Path;
	import javax.ws.rs.PathParam;
	import javax.ws.rs.Produces;
	import javax.ws.rs.core.Context;
	import javax.ws.rs.core.MediaType;
	
	import com.app.pojo.User;
	
	public interface IWebServiceRestful {
	
		@GET
		@Produces(MediaType.TEXT_PLAIN)
		public String doGet();
	
		@GET
		@Produces(MediaType.TEXT_PLAIN)
		@Path("/request/{param}")
		public String doRequest(@PathParam("param") String param, @Context HttpServletRequest servletRequest,
				@Context HttpServletResponse servletResponse);
	
		/*
		 * @Consumes：声明该方法使用 HTML FORM。
		 * 
		 * @FormParam：注入该方法的 HTML 属性确定的表单输入。
		 * 
		 * @Response.created(uri).build()： 构建新的 URI
		 * 用于新创建的联系人（/contacts/{id}）并设置响应代码（201/created）。 您可以使用
		 * http://localhost:8080/Jersey/rest/contacts/<id> 访问新联系人
		 */
		@POST
		@Path("/postData")
		public User postData() throws IOException;
	
		@PUT
		@Path("/putData/{id}")
		@Consumes(MediaType.APPLICATION_XML)
		public User putData(@PathParam("id") int id, User user);
	
		@DELETE
		@Path("/removeData/{id}")
		public void deleteData(@PathParam("id") int id);
	}

2.编写实现类

	package com.app.service.impl;
	
	import java.io.IOException;
	
	import javax.servlet.http.HttpServletRequest;
	import javax.servlet.http.HttpServletResponse;
	import javax.ws.rs.Consumes;
	import javax.ws.rs.DELETE;
	import javax.ws.rs.GET;
	import javax.ws.rs.POST;
	import javax.ws.rs.PUT;
	import javax.ws.rs.Path;
	import javax.ws.rs.PathParam;
	import javax.ws.rs.Produces;
	import javax.ws.rs.core.Context;
	import javax.ws.rs.core.MediaType;
	import javax.ws.rs.core.Request;
	import javax.ws.rs.core.UriInfo;
	
	import com.app.pojo.User;
	import com.app.service.IWebServiceRestful;
	
	
	public class WebServiceRestful implements IWebServiceRestful {
	
		@Context
	    private UriInfo uriInfo;
	    
	    @Context
	    private Request request;
		
		@GET
	    @Produces(MediaType.TEXT_PLAIN)
		@Path(value = "/sample")
	    public String doGet() {
	        return "this is get rest request";
	    }
		
	    @GET
	    @Produces(MediaType.TEXT_PLAIN)
	    @Path("/request/{param}")
	    public String doRequest(@PathParam("param") String param, 
	            @Context HttpServletRequest servletRequest, @Context HttpServletResponse servletResponse) {
	        System.out.println(servletRequest);
	        System.out.println(servletResponse);
	        System.out.println(param);
	        System.out.println(servletRequest.getParameter("param"));
	        System.out.println(servletRequest.getContentType());
	        System.out.println(servletResponse.getCharacterEncoding());
	        System.out.println(servletResponse.getContentType());
	        return "success";
	    }
	
		@Override
		@POST
		@Path("/postData")
		public User postData() throws IOException {
			System.out.println("postData");
			return null;
		}
	
		@Override
		@PUT
		@Path("/putData/{id}")
		@Consumes(MediaType.APPLICATION_XML)
		public User putData(int id, User user) {
			// TODO Auto-generated method stub
			return null;
		}
	
		@Override
		@DELETE
		@Path("/removeData/{id}")
		public void deleteData(int id) {
			// TODO Auto-generated method stub
		}
	}

3.配置application-cxf.xml

	<?xml version="1.0" encoding="UTF-8"?>
	<beans xmlns="http://www.springframework.org/schema/beans"
		xmlns:context="http://www.springframework.org/schema/context"
		xmlns:jaxws="http://cxf.apache.org/jaxws" xmlns:jaxrs="http://cxf.apache.org/jaxrs"
		xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		xsi:schemaLocation="http://www.springframework.org/schema/beans 
	    http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
	    http://www.springframework.org/schema/context
	    http://www.springframework.org/schema/context/spring-context-3.0.xsd
	    http://cxf.apache.org/jaxws 
	    http://cxf.apache.org/schemas/jaxws.xsd
	    http://cxf.apache.org/jaxrs
	    http://cxf.apache.org/schemas/jaxrs.xsd">
	
		<import resource="classpath*:META-INF/cxf/cxf.xml" />
		<import resource="classpath*:META-INF/cxf/cxf-extension-soap.xml" />
		<import resource="classpath*:META-INF/cxf/cxf-servlet.xml" />
	
		<bean id="inMessageInterceptor" class="org.apache.cxf.interceptor.LoggingInInterceptor" />
		<bean id="outLoggingInterceptor" class="org.apache.cxf.interceptor.LoggingOutInterceptor" />
		<!--rest风格-->
		<bean id="restSample" class="com.app.service.impl.WebServiceRestful" />
		<!-- 这里的地址很重要，客户端需要通过这个地址来访问WebService -->
		<jaxrs:server id="restServiceContainer" address="/rest">
			<jaxrs:serviceBeans>
				<ref bean="restSample" />
			</jaxrs:serviceBeans>
			<jaxrs:extensionMappings>
				<entry key="json" value="application/json" />
				<entry key="xml" value="application/xml" />
			</jaxrs:extensionMappings>
			<jaxrs:languageMappings>
				<entry key="en" value="en-gb" />
			</jaxrs:languageMappings>
		</jaxrs:server>
	
	</beans>

4.web.xml同普通方式

启动服务器后，根据path可获取需要的资源

### 实例

[我的MyWeb练习项目](https://github.com/Zering/MyWeb)