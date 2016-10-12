---
title: SpringMVC后台接受不到POST请求数据
date: 2016-10-10 10:45:42
tags: [springMVC, ajax]
---
[TOC]

## 序
项目代码合并更新之后遇到一个让我这几天及其崩溃的事情，Spring MVC的Controller里面使用了@RequestParam注解来接收参数，但是只在GET请求的时候才能正常访问，在使用POST请求的时候会产生找不到参数的异常。原本好好的POST请求开始报400错误，找不到REST服务，一般情况下报这种错误多是由于POST请求时传递的参数不一致，但是这次不存在这种问题，百思不得其解啊。。。网上各种资料显示，可能问题出现在jquery发送的ajax请求上。刚好恶补了解一下这方面内容。（PS：经过两天的折腾，尝试换Tomcat，IDE之后，发现是环境的问题，之前用的eclipse配置了破解的JRebel，可能影响到了，到第三天终于解决，神啊。。。）

## ajax提交
**jQuery**：发送的数据时用&符号连接起来的，如下：

![enter description here][1]
对应的Content-Type是 application/x-www-form-urlencoded。如下：

![enter description here][2]

**AngularJS**：传输数据使用的Content-Type是：application/json格式，对应的格式为{"pageNow":1,"pageSize":5,"type":""}

**数据的格式一定要和Content-Type保持一致。**

## form提交
form的enctype属性为编码方式，常用有两种：application/x-www-form-urlencoded和multipart/form-data，默认为application/x-www-form-urlencoded。 当action为get时候，浏览器用x-www-form-urlencoded的编码方式把form数据转换成一个字串（name1=value1&name2=value2...），然后把这个字串append到url后面，用?分割，加载这个新的url。 当action为post时候，浏览器把form数据封装到http body中，然后发送到server。 如果没有type=file的控件，用默认的application/x-www-form-urlencoded就可以了。 但是如果有type=file的话，就要用到multipart/form-data了。浏览器会把整个表单以控件为单位分割，并为每个部分加上Content-Disposition(form-data或者file),Content-Type(默认为text/plain),name(控件name)等信息，并加上分割符(boundary)。
## 注解
### @RequestBody
该注解用于读取Request请求的body部分数据
### @ResponseBody
该注解用于将Controller的方法返回的对象，通过适当的HttpMessageConverter转换为指定格式后，写入到Response对象的body数据区。
当返回的数据不是html的页面，而是其他某种格式的数据时（如json、xml等）使用；
### @RequestMapping produces

    @RequestMapping(value = "/upload",produces="application/json")  
produces：方法仅处理request请求中Accept头中包含了"application/json"的请求，同时暗示了返回的内容类型为application/json;

## springMVC取值方式
### @PathVariable
通过注解@PathVariable获取url中的值。

    @RequestMapping(value="user/{id}/{name}",method=RequestMethod.GET)
    public String myController(@PathVariable String id,@PathVariable String name, ModelMap model) {
         ……
          return "ok";
      }
### @RequestParam
通过注解RequestParam获取传递过来的值。

    @RequestMapping(value = "/test", method = RequestMethod.POST) 
    public String myTest(@RequestParam("name") String name,@RequestParam("phone") String phone, ModelMap model) { 
         ……
         return "ok";
     }
### HttpServletRequest
通过原生HttpServletRequest获取值。

    @RequestMapping(value="/test" method = RequestMethod.POST) 
    public String get(HttpServletRequest request, HttpServletResponse response) { 
         String name = request.getParameter("name")); 
         return "ok"; 
     }
### ModelAttribute
通过注解ModelAttribute直接映射表单中的参数到POJO。

## 补充
- **application/x-www-form-urlencoded**： 窗体数据被编码为名称/值对。这是标准的编码格式。空格转换为 "+" 加号，特殊符号转换为 ASCII HEX 值。
- **multipart/form-data**： 窗体数据被编码为一条消息，页上的每个控件对应消息中的一个部分。 不对字符进行编码，使用二进制数据传输，**一般用于上传文件**，非文本的数据传输。

Spring如果要接受这种数据，添加以下配置

    <bean id="multipartResolver"
          class="org.springframework.web.multipart.commons.CommonsMultipartResolver">       
    </bean>
- **text/plain**： 窗体数据以纯文本形式进行编码，其中不含任何控件或格式字符。

  [1]: ./images/Image%201.png "Image 1.png"
  [2]: ./images/Image%202.png "Image 2.png"