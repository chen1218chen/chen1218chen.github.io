---
title: chrome跨域报错
date: 2016-11-03 14:05:16
tags: 浏览器
---

ajax请求加载本地文件，chrome报跨域错误

    XMLHttpRequest cannot loadfile:///C:/Users/Li/Desktop/images/alist.json.Cross origin requests are only supported for protocol schemes: http, data,chrome-extension, https, chrome-extension-resource.

**解决方法：**
给chrome添加启动参数：`--allow-file-access-from-files`
**具体方法：**
在浏览器快捷方式上右键-属性-快捷方式-目标 如下图：

![enter description here][1]


  [1]: ./images/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20161103140925.png "微信截图_20161103140925.png"