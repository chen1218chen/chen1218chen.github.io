---
title: SSH配置集成
tags: SSH
---

[TOC]

## 工作流程
- web容器启动
- 加载spring配置文件，进行初始化，配置中包含hibernate配置信息

## Spring整合hibernate
- dataSource
- 配置sessionFactory，让spring来创建Session，注入dataSource。一般三个property：dataSource、packagesToScan和hibernateProperties
- 配置事务管理器 transactionManager，注入sessionFactory
- 对事务管理器管理事务设置注解方式  <tx:annotation-driven transaction-manager="transactionManager">
详见6  
配置文件的引入
  
	<context:property-placeholder location="classpath:jdc.properties">

## web.xml配置文件
- web.xml文件并不是web工程必须的。
- web.xml文件是用来初始化配置信息：比如Welcome页面、servlet、servlet-mapping、filter、listener、启动加载级别等。**在tomcat中的加载顺序是Context-Param，listener，filter，servlet**
- 每个xml文件都有定义它书写规则的Schema文件，也就是说javaEE的定义web.xml所对应的xml Schema文件中定义了多少种标签元素，web.xml中就可以出现它所定义的标签元素，也就具备哪些特定的功能。web.xml的模式文件是由Sun 公司定义的，每个web.xml文件的根元素为<web-app>中，必须标明这个web.xml使用的是哪个模式文件。
- ContextLoaderListener的作用就是启动Web容器时，自动装配ApplicationContext的配置信息。在web.xml配置这个监听器，启动容器时，就会默认执行它实现的方法。
### 基本配置###
		```	
		<!--增加struts2的配置-->
		<filter>
			<filter-name>struts2</filter-name>
			<filter-class>org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter</filter-class>
		</filter>
		<filter-mapping>
			<filter-name>struts2</filter-name>
			<url-pattern>*.action</url-pattern>
		</filter-mapping>
		
		<!--自动装配ApplicationContext的配置信息, Spring与Struts的整合 -->
		<listener>
			<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
		</listener>
		<context-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>classpath:applicationContext.xml</param-value>
		</context-param>
		
		```
### ssh中文过滤配置 ###
	<!-- ssh 中文过滤 -->
	<filter>
		<filter-name>Set Character Encoding</filter-name>
		<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
		<init-param>
			<param-name>encoding</param-name>
			<param-value>UTF-8</param-value>
		</init-param>
		<init-param>
			<param-name>forceEncoding</param-name>
			<param-value>true</param-value>
		</init-param>
	</filter>
	<filter-mapping>
		<filter-name>Set Character Encoding</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>
	<filter>
		<filter-name>struts-cleanup</filter-name>
		<filter-class>org.apache.struts2.dispatcher.ActionContextCleanUp</filter-class>
	</filter>
	<filter-mapping>
		<filter-name>struts-cleanup</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>

## struts.xml
  Struts2原理就是用拦截器，使得你客户端发送的请求都被拦截下来后处理。拦截器用到了反射机制。Struts2主要的功能是控制转发，在于Action的处理，和struts.xml配置。 

	```
	<?xml version="1.0" encoding="UTF-8" ?>
	<!DOCTYPE struts PUBLIC
	    "-//Apache Software Foundation//DTD Struts Configuration 2.0//EN"
	    "http://struts.apache.org/dtds/struts-2.0.dtd">
	
	<struts>
	<!-- 开启使用开发模式，详细错误提示 -->
		<constant name="struts.devMode" value="true" />
		<constant name="struts.i18n.encoding" value="utf-8"></constant>
		<constant name="struts.enable.DynamicMethodInvocation" value="true"></constant>
	
		<package name="default" extends="struts-default,json-default">
			<action name="getUnreadMessage" class="com.lbs.action.MessageAction"
				method="queryUnreadMessage">
				<result name="success" type="json">/success.jsp</result>
				<result name="error">/error.jsp</result>
			</action>
		</package>
	</struts>
	```

如果不配置，默认位置在WEB-INF文件夹底下。





