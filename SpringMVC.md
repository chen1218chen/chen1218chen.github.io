---
title: SpringMVC
date: 2016-04-19 16:35:41
tags: spring, springMVC
---

# SpringMVC #
## Spring控制器**DispatcherServlet**  
SpringMVC是一个基于DispatcherServlet的MVC框架,所有的请求都是首先提交给DispatcherServlet,DispatcherServlet负责转发每一个Request请求给相应的Handler。DispatcherServlet是继承自HttpServlet的。   
**web.xml**中配置

	<!-- 1.Spring配置 -->
	<listener>
	   <listenerclass>
	     org.springframework.web.context.ContextLoaderListener
	   </listener-class>
	</listener>

	<!-- 2.springMVC式rest -->
	<servlet>
		<servlet-name>rest</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>

		<!--可以自定义servlet.xml配置文件的位置和名称，默认为WEB-INF目录下，名称为[<servlet-name>]-servlet.xml，如rest-servlet.xml-->

		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>classpath:rest-servlet.xml</param-value>
		</init-param>
		<load-on-startup>1</load-on-startup>
	</servlet>
	<servlet-mapping>
		<servlet-name>rest</servlet-name>
		<url-pattern>/rest/*</url-pattern>
	</servlet-mapping>

	<!-- 3.指定Spring Bean的配置文件所在目录。默认配置在WEB-INF目录下 -->
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>classpath:applicationContext.xml</param-value>
	</context-param>
  
 以上是基本配置。    
##  rest-servlet.xml的配置  

	<?xml version="1.0" encoding="UTF-8"?>
	<beans xmlns="http://www.springframework.org/schema/beans"     
	       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:p="http://www.springframework.org/schema/p"     
	        xmlns:context="http://www.springframework.org/schema/context"     
	   xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-3.0.xsd   
	       http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-3.0.xsd   
	       http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-3.0.xsd   
	       http://www.springframework.org/schema/context <a href="http://www.springframework.org/schema/context/spring-context-3.0.xsd">http://www.springframework.org/schema/context/spring-context-3.0.xsd</a>">
	
	    <!-- 启用spring mvc 注解 -->
	    <context:annotation-config />
	
	    <!-- 设置使用注解的类所在的jar包 -->
	    <context:component-scan base-package="com.cc"></context:component-scan>
	
	    <!-- 完成请求和注解POJO的映射 -->
	    <bean class="org.springframework.web.servlet.mvc.annotation.AnnotationMethodHandlerAdapter" />
	　　
	    <!-- 对转向页面的路径解析。prefix：前缀， suffix：后缀 -->
	    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" p:prefix="/jsp/" p:suffix=".jsp" />
	</beans>
