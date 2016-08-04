---
title: '@FormParam与@RequestParam'
date: 2016-07-25 17:15:36
tags: [springMVC, REST, JAX-RS, Jersey]
---

@FormParam 是 Jersey（JAX-RS） 的
@RequestParam 是 SpringMVC 的

# RESTful
RESTful Web Service的实现方案有两种：
- 一种是按照JAX-RS规范的标准实现，
- 另外一种是按照自定义的方式实现。 

JAX-RS标准实现的框架比较有名的有：Jersey、Apache Wink等，其中，Jersey是JAX-RS规范的参考实现，比较有代表性。 

非标准实现的框架中比较有名的有：Spring MVC3.0、Restlet等，其中，Spring MVC3.0由于Spring框架在开发社区的影响力，使得它在非标准实现中比较有代表性，并且使用人群较多。 
另外、除了这些成熟的开发框架以外，要实现REST Web Service也可以利用 Apache URL重写机制，HTTPClient开发包，Java Servlet等方式实现。

# Jersey（JAX-RS）
JAX-RS即Java API for RESTful Web Services，是一个Java 编程语言的应用程序接口，支持按照表述性状态转移（REST）架构风格创建Web服务。
 
 ### @FormParam
 form传递的参数	接受form传递过来的参数。比如：@FormParam("name")  String userName