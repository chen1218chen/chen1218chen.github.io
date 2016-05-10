---
title: 深入了解HTML之meta
date: 2016-05-09 10:17:46
tags: html
---

## <!DOCTYPE>文档类型声明

<!DOCTYPE html>指示文档类型为html5类型，属于html标签。它是指示 web 浏览器关于页面使用哪个 HTML 版本进行编写的指令。对大小写不敏感。
[HTML 元素和有效的 DTD][1]查看标签的适用类型

## <meta>
定义关于 HTML 文档的元信息。标签属性是key-value对,key指http-equiv/name属性,value指content属性

    //页面重定向
    <meta http-equiv="Refresh" content="5;url=XX" />
    <meta name="referrer" content="origin" />
referer是由客户端的浏览器发送到服务器上，且在客户端可以通过document.referrer来获取上层页面，也就是说referer的发送实际上是一个浏览器行为，发送与否的决定权是在浏览器手里。
origin 是包含了schema和hostname的部分url不包含path等后面的其他url部分
项目中在login.html页面里设置这个，登录成功后的index.jsp页面中通过document.referrer无法获取到上层页面。
![enter description here][2]

## 移动网站
一个简单的移动网站meta标签可以设置为如下：表示强制让文档的宽度与设备的宽度保持1:1，并且文档最大的宽度比例是1.0，且不允许用户点击屏幕放大浏览

    <meta charset="utf-8">
    <meta content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0;"name="viewport"/>

## IE 兼容性
X-UA-Compatible是IE8特有的，它告诉IE8采用何种IE版本去渲染页面，对于ie8之外的浏览器是不识别的。

    //采用IE5
    <meta http-equiv="X-UA-Compatible" content="IE=5" />
为了避免制作出的页面在IE8下面出现错误，建议直接将IE8使用IE7进行渲染。也就是直接在页面的header的meta标签中加入如下代码：

    //采用IE7
    <meta http-equiv="X-UA-Compatible" content="IE=7" />
    //仿真IE7
    <meta http-equiv="X-UA-Compatible" content="IE=EmulateIE7" /> 
IE=edge告诉IE使用最新的引擎渲染网页，chrome=1则可以激活Chrome Frame
    
    <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" /> 
    
浏览器在https页面点击http连接的时候，不会在header加上referer。
    


  [1]: http://www.w3school.com.cn/tags/html_ref_dtd.asp
  [2]: ./images/Image%201.png "Image 1.png"