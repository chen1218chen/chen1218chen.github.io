---
title: ecplise导出maven工程依赖的jar包
date: 2018-03-12 14:27:32
tags: [maven, eclipse]
---
## ecplise导出maven工程依赖的jar包
最近SpringMVC项目中想要集成Swagger2插件，网上一搜资料大部分都是maven工程，直接配置POM.xml,添加如下配置

    <dependency> 
        <groupid>io.springfox</groupid> 
        springfox-swagger2</artifactid> 
        <version>2.4.0</version> 
        </dependency> 
        <dependency> 
        <groupid>io.springfox</groupid> 
        springfox-swagger-ui</artifactid> 
        <version>2.4.0</version> 
    </dependency>
无法知道都依赖哪些jar包，所以就尝试通过建立一个maven工程把需要的jar包下载下来后引入SpringMVC项目中，具体操作如下：
项目名称上右键点击，`run as->Maven build...`

![enter description here][1]

在打开的页面中，如图输入`“dependency:copy-dependencies”`，后点击“Run”即可

![enter description here][2]

在当前项目的目录的`“targed/dependency”`下就可以看见。

![enter description here][3]


  [1]: ./images/1481033-65e514a64372fb4e.png "1481033-65e514a64372fb4e.png"
  [2]: ./images/1481033-4458109ea068f79e.png "1481033-4458109ea068f79e.png"
  [3]: ./images/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20180312143702.png "微信截图_20180312143702.png"