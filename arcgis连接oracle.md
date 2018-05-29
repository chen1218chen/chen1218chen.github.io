---
title: arcgis连接oracle和服务发布
date: 2018-04-27 11:24:44
tags:[ arcgis, oracle ]
---
arcgis10.2的ArcMap是32位的，ArcGIS Server是64位的，所以导致需要oracle 64数据库服务，oracle client需要安装32位的。
oracle数据库安装完，如果不更改口令，默认超级管理员：sys，密码：change_on_install，普通管理员system，密码：manager
## 创建企业级地理数据库
打开ArcMap->打开Arctoolbox，如下图所示：

![enter description here][1]
填写数据库连接信息，sys默认密码：change_on_install

![enter description here][2]

点击确定，显示图下图所示框则连接成功
![enter description here][3]

## shp文件导入数据库
右键点击添加好的数据库连接，选择导入-> 要素类（单个）

![enter description here][4]
选择需要导入的shp文件，输出要素类名可以自定义，之后点击确定。

![enter description here][5]
成功之后显示下图弹框：

![enter description here][6]

## ArcGIS Server添加数据库

首先，添加GIS服务器

![enter description here][7]
然后，在添加好的服务器上右键点击服务器属性

![enter description here][8]
点击右侧"+"，进行数据库添加

![enter description here][9]
点击添加，填写数据库连接信息

![enter description here][10]
点击确定
## featureLayer图层发布
shp文件导入数据库后，就是数据库中的一张表，找到这张表，拖到左侧图层栏下后就可以点击文件->共享为->服务,选择发布服务

![enter description here][11]

![enter description here][12]

![enter description here][13]

![enter description here][14]

选择feature Access之后点击分析，如果分析结果没有问题发的话，直接点击发布进行服务的发布

![enter description here][15]


  [1]: ./images/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20180427131333.png "微信截图_20180427131333.png"
  [2]: ./images/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20180427134021.png "微信截图_20180427134021.png"
  [3]: ./images/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20180427134128.png "微信截图_20180427134128.png"
  [4]: ./images/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20180427134956.png "微信截图_20180427134956.png"
  [5]: ./images/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20180427135918.png "微信截图_20180427135918.png"
  [6]: ./images/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20180427135957.png "微信截图_20180427135957.png"
  [7]: ./images/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20180427140507.png "微信截图_20180427140507.png"
  [8]: ./images/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20180427140444.png "微信截图_20180427140444.png"
  [9]: ./images/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20180427140852.png "微信截图_20180427140852.png"
  [10]: ./images/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20180427140757.png "微信截图_20180427140757.png"
  [11]: ./images/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20180427140315.png "微信截图_20180427140315.png"
  [12]: ./images/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20180427141446.png "微信截图_20180427141446.png"
  [13]: ./images/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20180427141503.png "微信截图_20180427141503.png"
  [14]: ./images/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20180427141512.png "微信截图_20180427141512.png"
  [15]: ./images/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20180427141624.png "微信截图_20180427141624.png"