---
title: bower
date: 2016-06-07 16:11:25
tags: bower
---

# bower使用
前端的包管理器。
## 安装

     npm install -g bower
-g表示全局安装
npm是nodejs的默认包管理器

### bower init
在命令行中进入项目根目录，输入`bower init`,根据提示输入一些项目基本信息，或者直接回车或空格，会生成一个bower.json文件，用来保存该项目的配置
![enter description here][1]

### bower help
帮助命令
![enter description here][2]
### 包的管理

    bower install jquery
    bower uninstall jquery
包的更新，修改bower.json中的dependencies参数后执行如下命令：

    bower update 
bower会动更新包的版本  
上述命令完成以后，你会在你刚才创建的目录下看到一个bower_components的文件夹，其中目录包含所有安装的插件如下：
![enter description here][3]

      bower install jquery --save

其中--save参数是保存配置到你的bower.json，你会发现bower.json文件已经多了一行：

     "dependencies": {
        "jquery": "~2.1.4"
      }
### bower list
查看项目所安装的所有包。
![enter description here][4]
### 查找相关包
比如，需要搜索bootstrap相关的插件：

    bower search bootstrap
![enter description here][5]
### 包的信息
比如我们想要查找jquery都有哪些个版本，输入如下命令：

    bower info jquery
如下图说所示：
![enter description here][6]
...
![enter description here][7]


  [1]: ./images/3.png "3.png"
  [2]: ./images/7.png "7.png"
  [3]: ./images/2.png "2.png"
  [4]: ./images/1.png "1.png"
  [5]: ./images/6.png "6.png"
  [6]: ./images/4.png "4.png"
  [7]: ./images/5.png "5.png"