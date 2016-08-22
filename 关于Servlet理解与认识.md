---
title: 关于servlet理解与认识
tags: [java, servlet]
---
[TOC]
# 关于servlet理解与认识
## serlvet初识
一种运行于服务器端的java程序，用来与数据库进行交互，响应前台客户的请求，由web服务器加载并运行的。通俗来讲servlet就是用于后台开发，调用DAO
或者业务逻辑层来完成数据库操作，还可以用来接受表单参数和完成页面跳转。
Servlet技术是web开发中的一个核心部分，包括3各重要部分：**Servlet开发、Filter开发和Listener开发**。
前台和后台连接的一个桥梁。
JSP页面也是通过转译成servlet后才执行的。
## servlet开发
servlet本身就是一个简单的java类，只是所有的servlet类都必须继承**HttpServlet**，里面包含一个doGet、doPost方法
## servlet执行
servlet的执行需要在web.xml中配置一个servlet节点和servlet-mapping节点，分别用来配置servlet名称、所在类和通过节点名称为其指定的映射路径。

    <servlet>
        <servlet-name>HelloServlet</servlet-name>
        <servlet-class>com.servlet.HelloServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>HelloServlet</servlet-name>
        <url-pattern>/servlet/helloServlet</url-pattern>
    </servlet-mapping>
  
## servlet生命周期
1. 加载到servlet引擎
2. 调用init()方法初始化
3. 通过doGet()、doPost()方法处理客户端请求
4. 调用destory()方法进行销毁处理
5. 通过垃圾收集器进行收集清理
## Filter开发
Filter同servlet一样，也是一个java类，需要实现Filter接口,实现Filter接口中定义的init(),doFilter()和destroy()方法。
Filter的执行也需要向Servlet一样在web.xml中配置


    <Filter>
        <FilterFilter-name>HelloFilter</Filter-name>
        <Filter-class>com.servlet.HelloFilter</Filter-class>
    </Filter>
    <Filter-mapping>
        <Filter-name>HelloFilter</Filter-name>
        <url-pattern>/servlet/helloFilter</url-pattern>
    </Filter-mapping>
  
## Filter生命周期
1. 加载Filter
2. 调用init()方法初始化
3. 通过doFilter()方法处理客户端请求
4. 调用destory()方法进行销毁处理
5. 通过垃圾收集器进行收集清理  
