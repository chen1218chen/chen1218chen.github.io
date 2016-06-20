---
title: grunt + bower
date: 2016-06-08 09:46:40
tags: grunt
---
# [入门][1]
Grunt是JavaScript 任务运行器。对于需要重复执行的任务，例如压缩、编译、单元测试等，这种自动化工具可以减少你的工作量，使你的工作更轻松
Grunt和 Grunt 插件是通过 npm 安装并管理的。Grunt 0.4.x 必须配合Node.js >= 0.8.0版本使用。；奇数版本号的 Node.js 被认为是不稳定的开发版。
### 安装

    npm install grunt --save-dev
后面的 --save-dev 参数是说，把这个插件信息，同时添加到 package.json 的 devDependencies 中：
![enter description here][2]

由于grunt仅在开发阶段使用，所以使用 --save-dev 。如果是运行时使用的，则用 --save。
命令行下使用grunt：

    npm install -g grunt-cli
-g 是说把grunt-cli安装成全局工具。输入以下命令测试：
![enter description here][3]

并且已经有一份配置好package.json 和 Gruntfile 文件的项目了，至此grunt环境安装好，可以在项目中使用了
# 主要文件
一个grunt项目需要两个文件：package.json和Gruntfile.js，前者用于nodejs包管理，比如grunt插件安装，后者是grunt配置文件，配置任务或者自定义任务。
## package.json

package.json应当放置于项目的根目录中，与Gruntfile在同一目录中，并且应该与项目的源代码一起被提交。在上述目录(package.json所在目录)中运行npm install将依据package.json文件中所列出的每个依赖来自动安装适当版本的依赖。

下面列出了几种为你的项目创建package.json文件的方式：

    npm init/grunt-init
1. 大部分 grunt-init 模版都会自动创建特定于项目的package.json文件。
2. npm init命令会创建一个基本的package.json文件。
3. 复制下面的案例，并根据需要做扩充，参考此说明.


    {
      "name": "my-project-name",
      "version": "0.1.0",
      "devDependencies": {
        "grunt": "~0.4.5",
        "grunt-contrib-jshint": "~0.10.0",
        "grunt-contrib-nodeunit": "~0.4.1",
        "grunt-contrib-uglify": "~0.5.0"
      }
    }
## Gruntfile

Gruntfile.js 或 Gruntfile.coffee 文件是有效的 JavaScript 或 CoffeeScript 文件，应当放在你的项目根目录中，和package.json文件在同一目录层级，并和项目源码一起加入源码管理器。
为grunt创建配置文件Gruntfile.js，安装grunt-init

    # 安装grunt-init
    npm install grunt-init -g

    # 下载grunt模板
    git clone https://github.com/gruntjs/grunt-init-gruntfile.git ~/.grunt-init/gruntfile

    # 生成Gruntfile
    grunt-init gruntfile
根据需要回答问题，或者使用默认值，将得到Gruntfile.js文件
运行`grunt-init gruntfile`时，总报错，折腾好久才明白，被网上的这些资料误导了，`~`是Linux下表示当前目录的参数，如果用`~/.grunt-init/gruntfile`下载地址时，在windows环境下应该是：`grunt-init <PATH>`,PATH是template.js所在路径。

    grunt-init ~\.grunt-init\gruntfile
然后再根据提示生成gruntfile.js文件。
Gruntfile由以下几部分构成：

"wrapper" 函数
项目与任务配置
加载grunt插件和任务
自定义任务
## grunt-bower-task
bower只负责把依赖下载到本地的 bower_components 目录，并不负责把它们拷贝到我们项目中实际使用的地方，比如 public/js/lib 目录下。为了实现这样的功能，我们还需要另一个插件的帮助：

    npm install grunt-bower-task --save-dev
根目录下运行：

    # bower具体安装使用请查看《bower》一文
    npm install bower 
    # 将grunt与bower完美结合
    grunt bower
运行结果：
![enter description here][4]


  [1]: http://www.gruntjs.net/getting-started
  [2]: ./images/1.png "1.png"
  [3]: ./images/2.png "2.png"
  [4]: ./images/3.png "3.png"