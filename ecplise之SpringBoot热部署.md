---
title: ecplise之SpringBoot热部署
date: 2017-08-11 11:15:54
tags: [Spring Boot, eclipse,Gradle]
---
越来越体会到SpringBoot的强大好用，热部署是一个开发过程中必须要用到的功能，eclipse集成JRebel实现SpringMVC工程的热部署费了我九牛二虎之力才好,这次竟然能这么easy就设置成功例如，真是出乎意料啊，ღ( ´･ᴗ･` )。
Gradle项目中，首先安装配置依赖

    dependencies {
        compile group: 'org.springframework', name: 'springloaded', version:'1.2.6.RELEASE'
    }
    
其次，eclipse中需要在Run as->Run Configurations...中设置一下配置

    -javaagent:E:\mybatis\springloaded-1.2.6.RELEASE.jar -noverify

![enter description here][1]


最后重启项目搞定！

  [1]: ./images/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20170811111626.png "微信截图_20170811111626.png"