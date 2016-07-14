---
title: eclipse工程报错
date: 2016-06-16 14:46:07
tags: eclipse
---

工程导入eclipse中老有报错，经过各种折腾发现是java的原工程默认编译版本与eclipse环境下的编译版本不同。在工程根目录的.setting目录下的`org.eclipse.wst.common.project.facet.core.prefs.xml`文件中
![enter description here][1]
原文件为：
![enter description here][2]

修改为
![enter description here][3]
并且修改工程的编译版本
![enter description here][4]
统一使用JRE1.7版本

  [1]: ./images/Image%201.png "Image 1.png"
  [2]: ./images/Image%203.png "Image 3.png"
  [3]: ./images/Image%204.png "Image 4.png"
  [4]: ./images/Image%205.png "Image 5.png"