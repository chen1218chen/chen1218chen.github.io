---
title: content-type详解
date: 2017-04-24 11:12:11
tags: html
---
web项目开发中经常会遇到`content-type`属性的问题，尤其是关于一些中文乱码和浏览器端文件下载的问题中，解决方法一般都会需要在html页面上或者后台java代码中对content-type进行设置。查阅一些资料详细了解一下这个看着熟悉实则不太理解的属性。

MediaType，即是Internet Media Type，互联网媒体类型；也叫做MIME类型，在Http协议消息头中，使用Content-Type来表示具体请求中的媒体类型信息。

> 常见的媒体格式类型如下：

- text/html ： HTML格式
- text/plain ：纯文本格式      
- text/xml ：  XML格式
- image/gif ：gif图片格式    
- image/jpeg ：jpg图片格式 
- image/png：png图片格式
 
> 以application开头的媒体格式类型：

- application/xhtml+xml ：XHTML格式
- application/xml     ： XML数据格式
- application/atom+xml  ：Atom XML聚合格式    
- application/json    ： JSON数据格式
- application/pdf       ：pdf格式  
- application/msword  ： Word文档格式
- application/octet-stream ： 二进制流数据（如常见的文件下载）
- application/x-www-form-urlencoded ： <form  encType=””>中默认的encType，form表单数据被编码为key/value格式发送到服务器（表单默认的提交数据的格式）

> 另外一种常见的媒体格式是上传文件之时使用的：

- multipart/form-data ： 需要在表单中进行文件上传时，就需要使用该格式

以上就是我们在日常的开发中，经常会用到的若干content-type的内容格式。


[http请求 content-type对照表][1]
[springMVC rest请求中content-type的应用][2]


  [1]: http://tool.oschina.net/commons
  [2]: http://blog.csdn.net/blueheart20/article/details/45174399