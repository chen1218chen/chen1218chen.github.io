---
title: shiro实现SSL登陆
date: 2017-01-18 16:14:50
tags: shiro
---

考虑项目安全需求，将http请求转为https，项目中已集成的shiro框架已实现SSL登陆，来看下具体实现吧。

tomcat的server.xml文件配置。将http的8080端口调转到https的8443端口

    <Connector acceptCount="500" connectionTimeout="20000" enableLookups="false" maxThreads="400" port="8080" protocol="HTTP/1.1" URIEncoding="UTF-8" redirectPort="8443"/>

    <Connector SSLEnabled="true" acceptCount="500" clientAuth="false" keystoreFile="D:\localhost.keystore" keystorePass="aerors123" maxThreads="400" port="8443" protocol="HTTP/1.1" scheme="https" secure="true" sslProtocol="TLS"/>
    
applicationContext.xml中添加shiro相关配置

    <bean id="sslFilter" class="org.apache.shiro.web.filter.authz.SslFilter">
    		<property name="port" value="8443" />
    	</bean>

    <bean id="shiroFilter" class="org.apache.shiro.spring.web.ShiroFilterFactoryBean">
        <!-- securityManager -->
        <property name="securityManager" ref="securityManager" />
        <!-- 登录路径 -->
        <property name="loginUrl" value="/login.jsp" />
        <!-- 登录成功后跳转路径 -->
        <property name="successUrl" value="/index.jsp" />
        <!-- 授权失败跳转路径 -->
        <property name="unauthorizedUrl" value="/login.jsp" />
        <property name="filters">
        	<util:map>
        		<entry key="authc" value-ref="formAuthenticationFilter" />
        		<entry key="sysUser" value-ref="sysUserFilter" />
        		<entry key="kickout" value-ref="kickoutSessionControlFilter" />
        		<entry key="ssl" value-ref="sslFilter" />
        	</util:map>
        </property>
        <!-- 过滤链定义 -->
        <property name="filterChainDefinitions">
        	<value>
        		/login.jsp = ssl,anon
        		/logout = logout
        		/*.js = anon
        		/*.css = anon
        		/error.jsp= anon
        		/mapdaohang.jsp= anon
        		/unauthor.jsp= anon
        		<!-- authc表示需要认证的链接 -->
        		/lost.jsp = kickout,authc,roles["lost"]
        		/*.jsp = kickout,authc
        	</value>
        </property>
    </bean>

在filters中添加`<entry key="ssl" value-ref="sslFilter" />`即可在`filterChainDefinitions`中直接引用.

推荐个详细教程：[第十四章 SSL——《跟我学Shiro》][1]


  [1]: http://jinnianshilongnian.iteye.com/blog/2036420