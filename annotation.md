---
layout: hibernate
title: Spring注解
date: 2016-04-06 16:05:54
tags: spring
---

[TOC]

## model层
 1. catalog: 数据库名
 2. schema: postgreSQL数据库中的模式


    @Table(name = "category", catalog = "users")
    @Table(name = "roles", schema = "public")
## dao层（数据访问层，持久层）
**@Repository**
Spring2.0引入，为简化spring开发。用于将dao层的类标识为Spring Bean，同时为了Spring 扫描<context:component-scan / >时，识别出@Repository注解。所有标注了 @Repository 的类都将被注册为 Spring Bean。
**为什么 @Repository 只能标注在 DAO 类上呢？**  
这是因为该注解的作用不只是将类识别为Bean，同时它还能将所标注的类中抛出的数据访问异常封装为 Spring 的数据访问异常类型。 Spring本身提供了一个丰富的并且是与具体的数据访问技术无关的数据访问异常结构，用于封装不同的持久层框架抛出的异常，使得异常独立于底层的框架。
## service层和控制层
Spring2.5开始引入@Component、@Service、@Constroller，它们分别用于软件系统的不同层次：

@Service用于标注**业务层**组件（我们通常定义的service层就用这个）
@Controller用于标注**控制层**组件（如struts中的action）
@Repository用于标注**持久层**数据访问组件，即DAO组件
@Component **泛指组件**，当组件不好归类的时候，我们可以使用这个注解进行标注。

## @Resource与@service        
当需要定义某个类为一个bean，则在这个类的类名前一行使用@Service("XXX"),就相当于将这个类定义为一个bean，bean名称为XXX;   
当某类中需要引入这个对象时，则该对象定义的时候上面加@Resource。

    @Service
    public class UserServiceImpl implements IUserService {
    	@Resource
    	private IUserDao userDaoImpl;
    	@Resource
    	private IRolesDao rolesImpl;
    	
    	...
    }

　　@Resource的作用相当于@Autowired，只不过@Autowired按byType自动注入，而@Resource默认按 byName自动注入罢了。@Resource有两个属性是比较重要的，分是name和type，Spring将@Resource注解的name属性解析为bean的名字，而type属性则解析为bean的类型。所以如果使用name属性，则使用byName的自动注入策略，而使用type属性时则使用byType自动注入策略。如果既不指定name也不指定type属性，这时将通过反射机制使用byName自动注入策略。  
　　@Resource装配顺序  
　　1. 如果同时指定了name和type，则从Spring上下文中找到唯一匹配的bean进行装配，找不到则抛出异常   
　　2. 如果指定了name，则从上下文中查找名称（id）匹配的bean进行装配，找不到则抛出异常  
　　3. 如果指定了type，则从上下文中找到类型匹配的唯一bean进行装配，找不到或者找到多个，都会抛出异常  
　　4. 如果既没有指定name，又没有指定type，则自动按照byName方式进行装配；如果没有匹配，则回退为一个原始类型进行匹配，如果匹配则自动装配。
　
### spring bean
spring本身就是一个bean的工厂，完成bean的创建、配置。Spring容器集中管理bean。
bean的5种作用域：
1. Singleton：单例模式。在整个SpringIoC容器中，使用singleton定义的Bean将只有一个实例。
2. Prototype：原型模式。每次通过容器的getBean方法获取prototype定义的Bean时，都将产生一个新的Bean实例。
3. request：对于每次HTTP请求，使用request定义的Bean都将产生一个新的实例，即每次HTTP请求都会产生不同的Bean实例。当然只有在WEB应用中使用Spring时，该作用域才真正有效。
4. session：对于每次HTTPSession，使用session定义的Bean都将产生一个新的实例时，即每次HTTP 5.Session都将产生不同的Bean实例。同HTTP一样，只有在WEB应用才会有效。
global session：每个全局的HTTPSession对应一个Bean实例。仅在portlet Context的时候才有效。
常用singleton、prototype作用域。如果不指定Bean的作用域，则Spring会默认使用singleton作用域。
