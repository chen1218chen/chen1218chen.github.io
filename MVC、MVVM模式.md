---
title: "MVC、MVVM模式"
date: 2016-08-05 15:17:09
tags: MVC
---
[TOC]
# MVC、MVVM模式
## MVC
### 标准的MVC框架
一个标准的MVC框架中模型可以主动推送数据给视图进行更新（观察者模式），或者由控制器根据模型返回的数据选择合适的视图来展示如下图所示：

![enter description here][1]
- model: 包括业务逻辑、数据模型的集合，也定义数据修改和操作的业务规则。
- view： UI组件，例如CSS、HTML、JQuery等，负责从Controller接受数据，把model转化为UI。

- Controller： 负责处理流入数据，通过View接受用户输入，利用model处理数据，最后把数据返回给用户。

Model和View之间采用Observer模式。
Controller和View之间采用策略模式。
### WEB MVC框架
在WEB MVC框架中模型不能主动跟新用户界面。一般的web开发是请求-响应模式。例如：SpringMVC，详情请查看[SpringMVC][2]

![enter description here][3]
## MVVM模式
![enter description here][4]
- View和ViewModel数据进行双向绑定，使得ViewModel的状态改变可以自动传递给View。
- ViewModel通过observer模式来讲ViewModel的变化通知给Model。
如Angular，avalon(美团前端，司徒正美开发)


  [1]: ./images/2.png "2.png"
  [2]: http://chen1218chen.github.io/2016/04/20/SpringMVC/
  [3]: ./images/3.png "3.png"
  [4]: ./images/Image%202.png "Image 2.png"