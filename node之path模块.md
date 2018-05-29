---
title: node之path模块
date: 2018-05-23 16:42:03
tags: [node]
---

在node.js中，提供了一个path模块，专门用来处理路径的问题。
> 对window系统，目录分隔为'\\', 对于UNIX系统，分隔符为'/'
> `__dirname`:在任何模块文件内部，可以使用__dirname变量获取当前模块文件所在目录的完整绝对路径。

## node之path模块引用
    //引用该模块
    var path = require("path");
## path.resolve
获取绝对路径，以应用程序为起点，根据参数字符串解析出一个绝对路径

    var myPath = path.resolve('path1', 'path2', 'a/b\\c/');
    console.log(myPath);//E:\workspace\NodeJS\path1\path2\a\b\c

## path.join
该方法将多个参数值字符串结合成一个路径字符串

    var joinPath = path.join(__dirname, 'a', 'b', 'c');
    console.log(joinPath);      //   D:\nodePro\fileTest\a\b\c

## path.extname
获取路径中的扩展名，如果没有'.'，则返回空

