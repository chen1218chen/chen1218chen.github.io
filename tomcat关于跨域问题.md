---
title: tomcat关于跨域问题
date: 2016-05-13 16:53:48
tags: tomcat
---
##　tomcat解决跨域问题
web.xml中添加如下配置即可

    <filter>
      <filter-name>CorsFilter</filter-name>
      <filter-class>org.apache.catalina.filters.CorsFilter</filter-class>
      <init-param>
    	<param-name>cors.allowed.origins</param-name>
    	<param-value>*</param-value>
      </init-param>
      <init-param>
    	<param-name>cors.allowed.methods</param-name>
    	<param-value>GET,POST,HEAD,OPTIONS,PUT</param-value>
      </init-param>
      <init-param>
    	<param-name>cors.allowed.headers</param-name>
    	<param-value>Content-Type,X-Requested-With,accept,Origin,Access-Control-Request-Method,Access-Control-Request-Headers</param-value>
      </init-param>
      <init-param>
    	<param-name>cors.exposed.headers</param-name>
    	<param-value>Access-Control-Allow-Origin,Access-Control-Allow-Credentials</param-value>
      </init-param>
      <init-param>
    	<param-name>cors.support.credentials</param-name>
    	<param-value>true</param-value>
      </init-param>
      <init-param>
    	<param-name>cors.preflight.maxage</param-name>
    	<param-value>10</param-value>
      </init-param>
    </filter>
    <filter-mapping>
      <filter-name>CorsFilter</filter-name>
      <url-pattern>/*</url-pattern>
    </filter-mapping>