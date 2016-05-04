---
layout: hibernate
title: annotation
date: 2016-04-06 16:05:54
tags: annotation, hibernate
---

[TOC]

## model层
- catalog: 数据库名
- schema: postgreSQL数据库中的模式

    @Table(name = "category", catalog = "users")
    @Table(name = "roles", schema = "public")

## service层
###  @Repository、@Service 和 @Controller 注解
最好在持久层、业务层和控制层分别采用 @Repository、@Service 和 @Controller 对分层中的类进行注释 
 
### Spring中什么时候用@Resource，什么时候用@service        
当你需要定义某个类为一个bean，则在这个类的类名前一行使用@Service("XXX"),就相当于讲这个类定义为一个bean，bean名称为XXX;   
当需要在某个类中定义一个属性，并且该属性是一个已存在的bean，要为该属性赋值或注入时在该属性上一行使用@Resource(name="xxx")，相当于为该属性注入一个名称为xxx的bean。  
　　@Resource的作用相当于@Autowired，只不过@Autowired按byType自动注入，而@Resource默认按 byName自动注入罢了。@Resource有两个属性是比较重要的，分是name和type，Spring将@Resource注解的name属性解析为bean的名字，而type属性则解析为bean的类型。所以如果使用name属性，则使用byName的自动注入策略，而使用type属性时则使用byType自动注入策略。如果既不指定name也不指定type属性，这时将通过反射机制使用byName自动注入策略。  
　　@Resource装配顺序  
　　1. 如果同时指定了name和type，则从Spring上下文中找到唯一匹配的bean进行装配，找不到则抛出异常   
　　2. 如果指定了name，则从上下文中查找名称（id）匹配的bean进行装配，找不到则抛出异常  
　　3. 如果指定了type，则从上下文中找到类型匹配的唯一bean进行装配，找不到或者找到多个，都会抛出异常  
　　4. 如果既没有指定name，又没有指定type，则自动按照byName方式进行装配；如果没有匹配，则回退为一个原始类型进行匹配，如果匹配则自动装配。