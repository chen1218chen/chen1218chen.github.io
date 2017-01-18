---
title: URL路径含中文
date: 2017-01-18 16:01:59
tags:
---

项目最近碰到了一个以前从来没有注意到的问题，就是URL路径包含中文的问题，虽说不建议路径名中包含中文，但有的时候也不可避免的出现这种问题，找到一个管用的方法，修改tomcat的server.xml文件，添加 `URIEncoding="UTF-8"`属性。如下所示：

    <Connector acceptCount="500" connectionTimeout="20000" enableLookups="false" maxThreads="400" port="8080" protocol="HTTP/1.1" URIEncoding="UTF-8" redirectPort="8443"/>