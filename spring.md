---
title: spring
date: 2016-04-18 10:13:55
tags: spring
---

  
主页：http://spring.io/，进入projects，找到spring framework,进入的spring framework页，即可找到spring包及下载spring框架版本对jdk的要求

## 简单理解   
Spring其实就是一个管理框架的容器，你不用再考虑你new对象了，它会帮你做，降低了层与层之间的耦合度。Spring里面有很多的思想，IOC就是控制反转，注入。AOP是面向切面的，有点像拦截器。
## spring的jar包
Spring框架的jar包依赖于commons-logging.jar
如果是web项目就需要spring-web，在web.xml中配置的ContextLoaderListener监听器（listen）就需要用到spring-web。  
如果是web项目并且要使用spring mvc技术，就需要用到spring-webmvc
## spring技术介绍
Ioc: 依赖注入，解耦service层和DAO层
AOP: 切面开发
## 事务
事务是数据库中的概念，为了保证数据的一致和完整。是指单个逻辑功能单元执行的一系列操作，要么完整执行，要么都不执行。

 **四大特性**：  
- 原子性：事务必须是原子工作单元；对于其数据修改，要么全都执行，要么全都不执行；  
- 一致性：事务在完成时，必须使所有的数据都保持一致状态；  
- 隔离性：由并发事务所作的修改必须与任何其它并发事务所作的修改隔离；  
- 持久性：事务完成之后，它对于系统的影响是永久性的。

## Spring支持事务管理
Spring的事务管理分为声明式跟编程式。声明式就是在Spring的配置文件中进行相关配置；编程式就是用注解的方式写到代码里。
	
		```
		<!--开启注解方式-->
		<context:annotation-config />
		
		<!-- 配置sessionFactory -->
		<bean id="sessionFactory" class="org.springframework.orm.hibernate3.annotation.AnnotationSessionFactoryBean">
		    <property name="configLocation">
		        <value>classpath:config/hibernate.cfg.xml</value>
		    </property>
		    <property name="packagesToScan">
		        <list>
		            <value>com.entity</value>
		        </list>
		    </property>
		</bean>
		
		<!-- 配置事务管理器 -->
		<bean id="transactionManager" class="org.springframework.orm.hibernate3.HibernateTransactionManager">
		    <property name="sessionFactory" ref="sessionFactory"></property>
		</bean>
		
		<!-- 第四种配置事务的方式，注解 -->
		<tx:annotation-driven transaction-manager="transactionManager"/>
		```
## 注解配置  
 * **<context:annotation-config />** 是spring提供的注解的方式，注入使用时需要导入spring目录common-annotations.jar这个包。   
 * **<mvc:annotation-driven />** 3.1版本开始有这个配置 ,会自动注册DefaultAnnotationHandlerMapping与AnnotationMethodHandlerAdapter 两个bean,是spring MVC为@Controllers分发请求所必须的。  
 
<context:annotation-config> declares support for general annotations such as @Required, @Autowired, @PostConstruct, and so on.  

<mvc:annotation-driven /> is actually rather pointless. It declares explicit support for annotation-driven MVC controllers (i.e.@RequestMapping, @Controller, etc), even though support for those is the default behaviour.

My advice is to always declare <context:annotation-config>, but don't bother with <mvc:annotation-driven /unless you want JSON support via Jackson.  
 
## 事务配置  
在dao层类、方法上面添加@Transactional注解，为默认事务配置

	package com.dao;
	
	@Transactional
	public class UserDaoImpl_BAK extends HibernateTemplate {
	
	    @Transactional(propagation=Propagation.REQUIRED,rollbackForClassName="Exception")
	    public void addUser(User user) throws Exception {
	        this.save(user);
	    }
	
	    @Transactional(propagation=Propagation.REQUIRED,rollbackForClassName="Exception")
	    public void modifyUser(User user) {
	        this.update(user);
	    }
	
	    @Transactional(propagation=Propagation.REQUIRED,rollbackForClassName="Exception")
	    public void delUser(String username) {
	        this.delete(this.load(User.class, username));
	    }
	    
	    @Transactional(readOnly=true)
	    public void selectUser() {
	    }
	}    
 
## **applicationContext.xml** 公共配置

    <!-- 配置sessionFactory -->
    <bean id="sessionFactory" class="org.springframework.orm.hibernate3.annotation.AnnotationSessionFactoryBean">
        <property name="configLocation">
            <value>classpath:hibernate.cfg.xml</value>
        </property>
        <property name="packagesToScan">
            <list>
                <value>com.entity</value>
            </list>
        </property>
    </bean>
    
    <!-- 配置事务管理器（声明式的事务） -->
    <bean id="transactionManager" class="org.springframework.orm.hibernate3.HibernateTransactionManager">
        <property name="sessionFactory" ref="sessionFactory"></property>
    </bean>
    
    <!-- 配置DAO --> 
    <bean id="userDao" class="com.dao.UserDaoImpl">
        <property name="sessionFactory" ref="sessionFactory"></property>
    </bean>
