---
title: angular的ajax请求chrome跨域报错(XMLHttpRequest cannot loadfile:* Cross origin requests are only supported for protocol schemes)
date: 2016-11-03 14:05:16
tags: 浏览器
---
[TOC]
## 浏览器加参数"--allow-file-access-from-files"
ajax请求加载本地文件，chrome报跨域错误

    XMLHttpRequest cannot loadfile:///C:/Users/Li/Desktop/images/alist.json.Cross origin requests are only supported for protocol schemes: http, data,chrome-extension, https, chrome-extension-resource.

**解决方法：**
给chrome添加启动参数：`--allow-file-access-from-files`
**具体方法：**
在浏览器快捷方式上右键-属性-快捷方式-目标 如下图：

![enter description here][1]

## tomcat
部署到tomcat中去，通过`http://localhost:8080/XXX/#/index`来访问
## http-server
安装http-server，这是一个简单的零配置命令行HTTP服务器, 基于 nodeJs.

    npm install http-server -g
Windows 下使用:
在站点目录下开启命令行输入

    http-server
    
![enter description here][2]
访问: http://localhost:8081 or http://127.0.0.1:8081即可。

## firfox
再或者你懒得倒腾各种方法就换浏览器吧，用firfox就可以啦。

  [1]: ./images/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20161103140925.png "微信截图_20161103140925.png"
  [2]: ./images/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20161108111223.png "微信截图_20161108111223.png"