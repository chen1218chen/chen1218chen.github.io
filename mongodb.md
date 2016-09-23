---
title: mongodb
tags: mongodb
---

[TOC]

## 1. mongodb创建
安装好后，新建文件夹和文件如下图所示：

![enter description here][1]

![enter description here][2]
mongo.log文件为空

![enter description here][3]

mongo.config文件内容如下：

![enter description here][4]

    mongod --dbpath="mongodb安装目录\data" --logpath="mongodb安装目录\log\log.txt" --install --serviceName MongoDB --serviceDisplayName MongoDB

之后就可以在log.txt中看到
```
2015-10-23T14:51:17.374+0800 I CONTROL  [main] Hotfix KB2731284 or later update is not installed, will zero-out data files
2015-10-23T14:51:17.375+0800 I CONTROL  [main] Trying to install Windows service 'MongoDB'
2015-10-23T14:51:17.641+0800 I CONTROL  [main] Service 'MongoDB' (MongoDB) installed with command line '"C:\Program Files\MongoDB\Server\3.2\bin\mongod.exe" --dbpath=D:\mongodb\data\db --logpath=D:\mongodb\data\log\log.txt --service'
2015-10-23T14:51:17.641+0800 I CONTROL  [main] Service can be started from the command line with 'net start MongoDB'
```
之后就可以在服务管理里面看到

![enter description here][5]
可以在此手动启动，也可以在命令行窗口```net start MongoDB```启动

![enter description here][6]
这样服务就启动了。

    #关闭服务
    net stop mongodb
## 2. 使用

    cmd命令行里：
    mongo //进入数据库
    use hello-world //创建项目数据库
    db.addUser("shuaige", "123456") //给这个数据库创建了一个叫帅哥的账号，密码123456 （但是我觉得可能我理解的不到位，你也可以不做这个操作）
    然后，我们就为这个hello-world数据库创建collection（collection就相当于oracle和mysql里的table）
    db.createCollection("users") //创建一个集合，也就是表
    db.users.insert({userid: "admin", password: "123456"}) //给users里添加一个文档，也就是一条记录账号admin，密码123456
    ok，现在检查一下：
    db.users.find() //如果看到你刚刚添加的文档记录，就ok咯

在models目录下创建一个user.js,作为实体类映射数据库的users集合 

    var mongoose = require("mongoose");  //  顶会议用户组件
    var Schema = mongoose.Schema;    //  创建模型
    var userScheMa = new Schema({
        userid: String,
        password: String
    }); //  定义了一个新的模型，但是此模式还未和users集合有关联
    exports.user = mongoose.model('users', userScheMa); //  与users集合关联

- 注意：还要使用npm下载 mongoose模块

## 重装报错：无法找到系统文件
这几天因为项目的需要，重新安装了一下mongodb数据库，遇到很多新问题，记录一下吧，以方便后来查找。
重新安装mongodb后发现启动服务报错，无法找到系统文件，后来发现是因为重新安装mongodb的目录变化，服务还是老的路径，所以无法打开，直接用一下命令卸载老旧的服务

    mongod.exe --remove --serviceName "MongoDB"
如下图所示：

![enter description here][7]
然后运行一下命令，重新创建新的服务

    mongod --config D:\mongodb\mongo.config --install --serviceName "MongoDB"
查看新服务属性：

![enter description here][8]
可以看出可执行文件的路径对了，服务至此可以正常启动了。

## 修改搜索引擎
在2015/3/17以前，MongoDB只有一个存储引擎，叫做MMAP，MongoDB3.0的推出使得MongoDB有了两个引擎：MMAPv1和WiredTiger。

- MMAPv1：适应于所有MongoDB版本，MongoDB3.0的默认引擎
- WiredTiger：仅支持64位MongoDB
## collection无法显示
使用mongodb VUE 1.6.9.0可视化工具版本太低，对mongodb3.X支持度不好，网上有看到说是因为mongodb搜索引擎的问题，若使用MongoVUE这个工具，需要将存储引擎改成mmavp1，[mongoVUE中collections为空，即文件树无法展开问题的解决策略][9]，按照所说的方法没有成功朋友可以亲自尝试一下，下载使用最新的Romongo，mongodb的可视化工具，即可成功显示。


  [1]: ./images/Image%203.png "Image 3.png"
  [2]: ./images/Image%204.png "Image 4.png"
  [3]: ./images/Image%205.png "Image 5.png"
  [4]: ./images/Image%206.png "Image 6.png"
  [5]: ./images/1445583474158.png "1445583474158.png"
  [6]: ./images/1445583584543.png "1445583584543.png"
  [7]: ./images/Image%201.png "Image 1.png"
  [8]: ./images/Image%202.png "Image 2.png"
  [9]: http://blog.csdn.net/qq_33279781/article/details/52047564