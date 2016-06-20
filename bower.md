---
title: bower
date: 2016-06-07 16:11:25
tags: bower
---

# bower使用
前端的包管理器。
Bower是基于Git之上的包管理工具，提供的包的源头都是Git库（多数在Github上，并非所有）。能够通过Bower下载的包，在其Git库下都会有一个bower.json文件。
我们在bower上下载的包，大部分都是包的源代码库，有一大堆没有编译或者构建过的半成品文件，对于我们使用来说比较多余。为了只获取我们需要的文件，经常使用的工具是Grunt，一种任务运行器，常被用作前端项目构建工具，使用bower的项目几乎没有不使用Grunt或其他类似工具的。
bower只负责把依赖下载到本地的bower_components目录，并不负责把它们拷贝到我们项目中实际使用的地方，比如public/js/lib目录下。

为了实现这样的功能，我们还需要另一个插件的帮助：

    npm install grunt-bower-task --save-dev
首先在Gruntfile中添加

    grunt.loadNpmTasks('grunt-bower-task');
然后在grunt.initConfig({...})参数中，添加相应的配置项：

    bower: {
        install: {
          options: {
            targetDir: './public/js/lib',
            layout: 'byComponent',
            install: true,
            verbose: false,
            cleanTargetDir: false,
            cleanBowerDir: false,
            bowerOptions: {}
          }
        }
      }
这里指定拷贝的目标目录为public/js/lib，且文件按照模块分成单个目录(byComponent)。如果想把所有的js放在同一个目录，所有的css文件放在同一个目录，则使用byType
## 安装

     npm install -g bower
-g表示全局安装
npm是nodejs的默认包管理器
### 常用的插件

    bower install jquery#1.11.1 –save
    bower install bootstrap –save
    bower install bootstrap-table –save
    bower install font-awesome -save
    bower install d3 –save
    bower install jqueryui –save
    bower install datatables#1.10.2 –save
    bower install echarts –save
    bower install moment –save
    bower install backbone –save
    bower install seajs –save
    bower install requirejs -save
    bower install angular -save
### download 方式

    # 已注册的包，使用简写即可
    $ bower install jquery
    # GitHub 上的项目，使用名称即可
    $ bower install desandro/masonry
    # GitHub上的项目
    $ bower install git://github.com/user/package.git
    # 直接通过 URL 下载
    $ bower install http://example.com/script.js
参数：

    -F, –force-latest: 不管冲突问题强制使用最新版本
    -p, –production: 安装生产环境的库，不安装开发环境所需的文件
    -S, –save: 将安装的包信息保存到项目的 bower.json 依赖配置中
    -D, –save-dev: 将已安装的包信息保存到项目开发环境的 bower.json 依赖中
## 命令
### bower init
创建bower.json文件。
在命令行中进入项目根目录，输入`bower init`,根据提示输入一些项目基本信息，或者直接回车或空格，会生成一个bower.json文件，用来保存该项目的配置
![enter description here][1]

### bower help
帮助命令
![enter description here][2]
### bower install/uninstall
包的管理

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
查看本地缓存包

    bower cache list
### bower search
查找相关包
比如，需要搜索bootstrap相关的插件：

    bower search bootstrap
![enter description here][5]
### bower info
包的信息
比如我们想要查找jquery都有哪些个版本，输入如下命令：

    bower info jquery
如下图说所示：
![enter description here][6]
...
![enter description here][7]
### 查看库的url

    bower lookup jquery
### 查看库的主页

    bower home jquery

## 注册
先在github上创建资源库，将本地工程通过`git push origin master`上传，然后注册：

    bower register 工程名 github地址
之后就可以通过`bower install`来安装了。

  [1]: ./images/3.png "3.png"
  [2]: ./images/7.png "7.png"
  [3]: ./images/2.png "2.png"
  [4]: ./images/1.png "1.png"
  [5]: ./images/6.png "6.png"
  [6]: ./images/4.png "4.png"
  [7]: ./images/5.png "5.png"
