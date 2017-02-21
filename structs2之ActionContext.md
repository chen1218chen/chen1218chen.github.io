---
title: structs2之Action与ActionContext
date: 2017-02-21 10:10:30
tags: [struts2, servlet]
---

[TOC]

## Action
structs2的核心功能就是action，主要是编写一个个action来处理逻辑。
action类通常都要实现`com.opensymphony.xwork2.Action`接口，并实现该接口中的`execute()`方法。

Struts2并不是要求所有编写的action类都要实现Action接口，也可以直接编写一个普通的Java类作为action，只要实现一个返回类型为String的无参的public方法即可：`public String  xxx()`。

在实际开发中，action类很少直接实现Action接口，通常都是从`com.opensymphony.xwork2.ActionSupport`类继承，ActionSupport实现了Action接口和其他一些可选的接口，提供了输入验证，错误信息存取，以及国际化的支持，选择从ActionSupport继承，可以简化action的定义。


>项目中常用的action基类，封装了对`HttpServletRequest、HttpServletResponse、HttpSession`的获取，所有的action都从baseAction继承。

    public class BaseAction extends ActionSupport{
        protected final transient Log log = LogFactory.getLog(getClass());
        
          /**
         * 获取请求
         * Convenience method to get the request
         * @return current request
         */
        protected HttpServletRequest getRequest() {
            return ServletActionContext.getRequest();
        }

        /**
         * 获取回应
         * Convenience method to get the response
         * @return current response
         */
        protected HttpServletResponse getResponse() {
            return ServletActionContext.getResponse();
        }

        /**
         * 获取session
         * @return
         */
        protected HttpSession getSession() {
            return getRequest().getSession();
        }
    }

    @ParentPackage(value = "json-default")
    @Controller
    @Scope("prototype")
    @Action(value = "/loginAction", results = { @Result(name = "success", type = "redirect",location = "/index.jsp"),
    @Result(name = "input", type = "redirect", location = "/login.jsp"),
    @Result(name = "error", type = "redirect", location = "/login.jsp") }, exceptionMappings = {
    		@ExceptionMapping(exception = "java.lang.Exception", result = "error") })
    public class LoginAction extends BaseAction {
        ...
        public String login() throws Exception {
            ...
            HttpServletRequest request= getRequest(); 
            ...
            return SUCCESS;
        }
        
         public String logout() throws Exception {
            ...
            HttpServletRequest request= getRequest(); 
            ...
            return SUCCESS;
        }
    }

开发好action之后，好需要对action进行配置，以告诉Struts2框架，针对某个URL的请求应该交由哪个action进行处理。`@Action`可以写在类名上，也可以写在方法名上。
继承baseAction类的action类中则可以直接`getRequest()、getResponse()、getSession()`直接获取到Http请求或者响应。
## Action原型 
**Prototype 原型**    每次请求都会创建一个新的Action对象，俗称“多例”。一个注册的例子,如果没加上这个注解,注册完成后,下一个请求再注册一次,Action里会保留上一次注册的信息..    
**Singleton 原型：**    当第一次请求时，创建servlet对象之后每次都使用该对象即可。spring 默认scope 是单例模式

    @Scope("prototype")
添加@Scope注解，如上`LoginAction`类所示。
## ActionContext
在Web应用程序开发中,除了将请求参数自动设置到Action的字段中,我们往往也需要在Action里直接获取请求(Request)或会话 (Session)的一些信息, 甚至需要直接对JavaServlet Http的请求(HttpServletRequest),响应(HttpServletResponse)操作。

    ActionContext context = ActionContext.getContext();
    Map params = context.getParameters();
    String username = (String) params.get("username");
    Map session = context.getSession();
ActionContext(`com.opensymphony.xwork.ActionContext`)是Action执行时的上下文,上下文可以看作是一个容器(其实我们这里的容器就是一个Map而已),它存放放的是Action在执行时需要用到的对象。

## ServletActionContext
ServletActionContext(`com.opensymphony.webwork. ServletActionContext`),这个类直接继承了我们上面介绍的ActionContext,它提供了直接与Java Servlet相关对象访问的功能,它可以取得的对象有:

 1. javax.servlet.http.HttpServletRequest:HTTPservlet请求对象 
 2. javax.servlet.http.HttpServletResponse;:HTTPservlet相应对象 
 3. javax.servlet.ServletContext:Servlet 上下文信息 
 4. javax.servlet.ServletConfig:Servlet配置对象  
 5. javax.servlet.jsp.PageContext:Http页面上下文
> 从ServletActionContext里取得Servlet的相关对象:


    HttpServletRequest request = ServletActionContext. getRequest();
    HttpSession session = ServletActionContext.getRequest().getSession();
如上baseAction类所示。
## 线程安全
ActionContext是线程安全的，而ServletActionContext继承自ActionContext，所以ServletActionContext也线程安全，线程安全要求每个线程都独立进行，所以req的创建也要求独立进行，所以`ServletActionContext.getRequest()`这句话不要放在构造函数中，也不要直接放在类中，而应该放在每个具体的方法体中(eg：login()、queryAll()、insert()等)，这样才能保证每次产生对象时独立的建立了一个request。

如上LoginAction类所示，`HttpServletRequest request= getRequest();`放在login(),logout()方法体里面，不能放在之外。
## servlet和Structs2
刚好抽空总结一下这些知识点，项目开发中就会更加清晰，说道这里就难免想到servlet，毕竟Structs2是在servlet上发展起来的。看到一个很不错的分析关于servlet和Structs2，分享一下。
### servlet分析
 客户端--->web容器-->web.xml-->servlet来处理 ----->model-->数据库
### structs2分析
客户端----->web容器--->web.xml-->struts2过滤器--->struts.xml--->Action--->model--->数据库

