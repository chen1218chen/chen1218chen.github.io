---
title: SpringMVC中集成Sigar
date: 2017-07-06 09:15:10
tags:
---

`sigar:`系统信息收集和报表工具，开源的，用来收集服务器的CPU、内存、磁盘存储、网络监控等数据，除了需要在工程中引入`sigar.jar`包外，还需要在jdk的bin目录(`D:\Program Files\Java1.7\jdk1.7.0_80\bin`)下放`sigar-amd64-winnt.dll`和`sigar-x86-winnt.dll`动态库。否则会报错，拿不到需要的服务器数据。

![enter description here][1]


  [1]: ./images/1499304165210.jpg "1499304165210.jpg"