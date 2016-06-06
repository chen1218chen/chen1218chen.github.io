---
title: SpringMVC
date: 2016-04-19 16:35:41
tags: spring, springMVC
---


## 标准的MVC框架
一个标准的MVC框架中模型可以主动推送数据给视图进行更新（观察者模式），或者由控制器根据模型返回的数据选择合适的视图来展示如下图所示：

![enter description here][1]
## WEB MVC框架
在WEB MVC框架中模型不能主动跟新用户界面。一般的web开发是请求-响应模式。

![enter description here][3]
## SpringMVC
SpringMVC框架基于WEB MVC的一个轻量级框架，转发过程图：

![enter description here][2]
![enter description here][4]
涉及到几个重要的处理过程：
- DispatcherServlet
- HandlerMapping
- HandleAdapter
- ViewResolver

> Question：
> 1. 请求如何发到DispatcherServlet(前端控制器)？   
web.xml中配置DispatcherServlet（如下的步骤2），即可拦截请求。
> 2. DispatcherServlet如何根据请求信息选择不同的处理器进行处理？ HandlerMapping
> 3. 如何支持多种页面处理器？ 
HandleAdapter（适配器模式）


## Spring控制器**DispatcherServlet**  
SpringMVC是一个基于DispatcherServlet的MVC框架,所有的请求都是首先提交给DispatcherServlet,DispatcherServlet负责转发每一个Request请求给相应的Handler。DispatcherServlet是继承自HttpServlet的。   
**web.xml**中配置

    <!-- 1.Spring配置 -->
    <listener>
        <listenerclass>
            org.springframework.web.context.ContextLoaderListener
        </listener-class>
    </listener>
ContextLoaderListener初始化的上下文加载的Bean是对于整个应用程序共享的
，Spring监听器。会自动找到所有配置在applicationContext.xml中的Spring Beans

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
 `<url-pattern>`表示哪些请求交给Spring Web MVC处理， “/” 是用来定义默认servlet映射的
##  rest-servlet.xml的配置  
```
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
	　　
	    <!-- 对ModelAndView中的转向页面的路径解析，自动路径添加prefix：前缀， suffix：后缀 -->
	    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" p:prefix="/jsp/" p:suffix=".jsp" />
	</beans>
```


  [1]: ./images/2.png "2.png"
  [2]: ./images/4.png "4.png"
  [3]: ./images/3.png "3.png"
  [4]: ./images/1.png "1.png"