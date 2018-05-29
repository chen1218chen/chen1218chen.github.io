---
title: eclipse集成maven
date: 2018-04-18 16:42:03
tags: [eclipse, maven]
---
## eclipse配置

打开window->prefenerces->maven配置项

![enter description here][1]
在`Installations`中点击add添加maven主目录

![enter description here][2]
再在`User Settings`配置项中，在`Global Settings`和`User Settings`中添加maven的配置文件路径

![enter description here][3]

## setting.xml
最后，修改`User Settings`中添加的maven配置文件，

![enter description here][4]

在setting.xml中`mirrors`节点下添加子节点，如下所示：

    <mirror>
      <id>tomcat</id>
      <mirrorOf>*</mirrorOf>
      <name>Nexus aliyun</name>
      <url>http://192.168.1.194:8081/repository/maven-public/</url>
    </mirror>

> url路径为本地中央仓库地址

这样，eclipse中集成maven就完成啦，mark一下，以后就不会忘记啦！！！

  [1]: ./images/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20180418165424.png "微信截图_20180418165424.png"
  [2]: ./images/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20180418165443.png "微信截图_20180418165443.png"
  [3]: ./images/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20180418165501.png "微信截图_20180418165501.png"
  [4]: ./images/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20180418164232.png "微信截图_20180418164232.png"