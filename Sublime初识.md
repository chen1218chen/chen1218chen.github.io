---
title: Sublime初识
date: 2016-09-05 10:24:45
tags: Sublime
---
[TOC]

这款编辑器已经被安利很久了，总算是开始使用了，果然开局总是很不顺利的，各种问题啊。。。跟大家分享一下解决方法吧。
## emmet的安装
>首先安装Package Control：
1. Ctrl+~打开控制台，在控制台输入如下的Python命令


    import  urllib.request,os;pf='Package Control.sublime-package';ipp=sublime.installed_packages_path();urllib.request.install_opener(urllib.request.build_opener(urllib.request.ProxyHandler()));open(os.path.join(ipp,pf),'wb').write(urllib.request.urlopen('http://sublime.wbond.net/'+pf.replace(' ','%20')).read())
2. 重启后，如果在Preferences->package settings中有看到package control这一项，则安装成功。
>安装emmet
1. Ctrl+Shift+P：调出控制面板
2. 输入Install Package，并回车
3. emmet安装

### PyV8的问题
如果提示需安装PyV8，则需要把PyV8下载下来解压到

![enter description here][1]
这个地址可以通过Preferences->Browse Packages直接进入以免不好找啦
具体内容如下：

![enter description here][2]

再次重启一下就ok可以使用了，初次体验感觉很好，果然可以大大提高编码速度，熟悉之后可以大幅度简化代码啊，nice！

  [1]: ./images/Image%206.png "Image 6.png"
  [2]: ./images/Image%207.png "Image 7.png"