---
title: Angularjs + Express3 + Bootstrap3 
tags: [node.js, Angular.js, Express3, Bootstrap]
grammar_cjkRuby: true
---

[TOC]



## 环境搭建
**Angularjs + Express3 + Bootstrap3**
angularjs是由Google团队开发的一款非常优秀web前端框架。Bootstrap让界面美观大方，对于不懂UE的人，也能做出专业级的水准。再结合Nodejs的Express做后端，三剑合并，太无敌了，大有统一前端开发的趋势，前途不可估量！

### 1. 首先安装node.js
### 2.  webStorm(node.js最好的IDE开发工具)
### 3.  express 安装
Express 是一个简洁而灵活的 node.js Web应用框架, 提供了一系列强大特性帮助你创建各种 Web 应用，和丰富的 HTTP 工具。
```
npm install express --save

```
以上命令会将 Express 框架安装在当前目录的 node_modules 目录中， node_modules 目录下会自动创建 express 目录。以下几个重要的模块是需要与 express 框架一起安装的：
- body-parser - node.js 中间件，用于处理 JSON, Raw, Text 和 URL 编码的数据。
- cookie-parser - 这就是一个解析Cookie的工具。通过req.cookies可以取到传过来的cookie，并把它们转成对象。
- multer - node.js 中间件，用于处理enctype="multipart/form-data"（设置表单的MIME编码）的表单数据。


    $ cnpm install body-parser --save
    $ cnpm install cookie-parser --save
    $ cnpm install multer --save

安装成功后在node_modules下会找到express目录，同时也会找到.bin目录，它里面有express命令脚本，在终端下执行
```
express project_name
```
### 4. Bower安装
bower是一个客户端技术的软件包管理器，它可用于搜索、安装和卸载如JavaScript、HTML、CSS之类的网络资源。**安装依赖：NodeJS、NPM、Git**

![Alt text](./1444459191394.png)

安装包是可使用
```
bower install <package>
bower install jquery
```
其中包的形式的可以是GitHub简写、一个git地址、一个url或者其他。
```
# registered package
$ bower install jquery
# GitHub shorthand
$ bower install desandro/masonry
# Git endpoint
$ bower install git://github.com/user/package.git
# URL
$ bower install http://example.com/script.js
```
如果安装成功，则目录中会出现bower_components子目录，其中包括了下载的jQuery源文件。

![Alt text](./1444461188031.png)

卸载包可以使用uninstall 命令：
```
$ bower uninstall jquery
```

 Q：安装bower时，报Bower : ENOGIT git is not installed or not in the PATH错误？
 1. windows中
需要配置你的Git到path，假如你的git安装目录是”C:\Program Files (x86)\Git”，在path中加入git的bin和cmd目录，如C:\Program Files (x86)\Git\bin;C:\Program Files (x86)\Git\cmd
 2. Ubuntu 中
```
 $ apt-get install git
```
### 5. angular、bootstrap安装
```
bower install angular
bower install angular-route
bower install bootstrap
```

## web开发
Express + EJS + Mongoose/MySQL，通常用Nodejs做Web开发，需要这3个框架配合使用，就像Java中的SSH。
- express 是轻量灵活的Nodejs Web应用框架
- ejs是一个嵌入的Javascript模板引擎，通过编译生成HTML的代码。
- mongoose 是MongoDB的对象模型工具，通过Mongoose框架，可以进行访问MongoDB的操作。

--------------------------
**应用实例：**

 Web聊天室(IM)：Express + Socket.io
　　socket.io一个是基于Nodejs架构体系的，支持websocket的协议用于时时通信的一个软件包。socket.io 给跨浏览器构建实时应用提供了完整的封装，socket.io完全由javascript实现。

------------------------------
　

## 第一个web工程
1. 新建工程

![Alt text](./1444454468507.png)


创建成功后入下图所示：
![Alt text](./1444454617120.png)
**app.js：**是项目的入口文件
**node_modules/：** 项目的依赖库。存放npm安装到本地依赖包，依赖包在package.json文件中声明，使用npm install指令安装。
**package.json：**  npm依赖配置文件， 类似ruby中的Gemfile, java Maven中的pom.xml文件. 一会需要在这里添加 markdown-js 项目依赖。
**public/：** 存放静态资源文件, jquery/prettify.js等静态库会方这里，当然自己编写的前端代码也可以放这里
**view/：** 模板文件, express默认采用jade, 当然，你也可以使用自己喜欢的haml,JES, coffeeKup, jQueryTemplate等模板引擎
**routes：** 路由文件(学习的重要攻克对象。业务好不好，路由是关键)
2.  目录结构
- public 公共插件
- routers 业务逻辑
- views 页面展示
