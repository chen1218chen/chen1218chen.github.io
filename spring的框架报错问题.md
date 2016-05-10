---
title: org.springframework.beans.factory.BeanDefinitionStoreException
date: 2016-05-05 10:37:37
tags: spring
---


## org.springframework.beans.factory.BeanDefinitionStoreException

	[ERROR] [10:30:28] org.springframework.web.context.ContextLoader - Context initialization failed
    org.springframework.beans.factory.BeanDefinitionStoreException: IOException parsing XML document from class path resource [applicationContext.xml]; nested exception is java.io.FileNotFoundException: class path resource [applicationContext.xml] cannot be opened because it does not exist
	at org.springframework.beans.factory.xml.XmlBeanDefinitionReader.loadBeanDefinitions(XmlBeanDefinitionReader.java:341)
	at org.springframework.beans.factory.xml.XmlBeanDefinitionReader.loadBeanDefinitions(XmlBeanDefinitionReader.java:302)
	at org.springframework.beans.factory.support.AbstractBeanDefinitionReader.loadBeanDefinitions(AbstractBeanDefinitionReader.java:174)
	at org.springframework.beans.factory.support.AbstractBeanDefinitionReader.loadBeanDefinitions(AbstractBeanDefinitionReader.java:209)
	at org.springframework.beans.factory.support.AbstractBeanDefinitionReader.loadBeanDefinitions(AbstractBeanDefinitionReader.java:180)
	at org.springframework.web.context.support.XmlWebApplicationContext.loadBeanDefinitions(XmlWebApplicationContext.java:125)
	at org.springframework.web.context.support.XmlWebApplicationContext.loadBeanDefinitions(XmlWebApplicationContext.java:94)
	at org.springframework.context.support.AbstractRefreshableApplicationContext.__refreshBeanFactory(AbstractRefreshableApplicationContext.java:131)
	at org.springframework.context.support.AbstractRefreshableApplicationContext.refreshBeanFactory(AbstractRefreshableApplicationContext.java)
	at org.springframework.context.support.AbstractApplicationContext.obtainFreshBeanFactory(AbstractApplicationContext.java:522)
	at org.springframework.context.support.AbstractApplicationContext.__refresh(AbstractApplicationContext.java:436)
	at org.springframework.context.support.AbstractApplicationContext.refresh(AbstractApplicationContext.java)
	at org.springframework.web.context.ContextLoader.configureAndRefreshWebApplicationContext(ContextLoader.java:385)
	at org.springframework.web.context.ContextLoader.initWebApplicationContext(ContextLoader.java:284)
	at org.springframework.web.context.ContextLoaderListener.contextInitialized(ContextLoaderListener.java:111)
	at org.apache.catalina.core.StandardContext.listenerStart(StandardContext.java:5017)
	at org.apache.catalina.core.StandardContext.startInternal(StandardContext.java:5531)
	at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:150)
	at org.apache.catalina.core.ContainerBase$StartChild.call(ContainerBase.java:1574)
	at org.apache.catalina.core.ContainerBase$StartChild.call(ContainerBase.java:1564)
	at java.util.concurrent.FutureTask.run(Unknown Source)
	at java.util.concurrent.ThreadPoolExecutor.runWorker(Unknown Source)
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(Unknown Source)
	at java.lang.Thread.run(Unknown Source)
	Caused by: java.io.FileNotFoundException: class path resource [applicationContext.xml] cannot be opened because it does not exist
	at org.springframework.core.io.ClassPathResource.getInputStream(ClassPathResource.java:158)
	at org.springframework.beans.factory.xml.XmlBeanDefinitionReader.loadBeanDefinitions(XmlBeanDefinitionReader.java:328)
	... 23 more

applicationContext.xml文件无法找到，在web.xml中配置
	
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>classpath:applicationContext.xml</param-value>
	</context-param>	
再查看一下，tomcat发布后的classes下有没有这个文件。
eclipse发布后的工程默认不在tomcat的webapps中，需要手动设置。
![enter description here][1]
将Deploy path修改为webapps（最好是使用完整的目录，如：D:\apache-tomcat-7.0.63\webapps），默认是wtpwebapps。
若server Location无法点击选择，则右击add and remove...把项目移除，在右击clean即可。


  [1]: ./images/Image%201.png "Image 1.png"