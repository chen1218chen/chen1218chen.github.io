---
title: tomcat发布后访问路径问题
date: 2016-10-27 14:33:29
tags: tomcat
---
近来一直发现一个问题，tomcat工程发布后路径不对，用http://localhost:8080/Urban/login.jsp报404，无法访问。
>这是tomcat的server.xml文件配置

    <Context docBase="chengguanfuwuqi" path="/Urban" reloadable="false" source="org.eclipse.jst.jee.server:chengguanfuwuqi"/></Host>
查阅资料都说，docBase是工程的存放路径，可以写成绝对路径，相对路径是指%TOMCAT_HOME%/webapps/下，path是指虚拟的访问路径。这么说来http://localhost:8080/Urban/login.jsp是没有问题的。

如果还是出现404的错误，且前面配置没有错，可能就是在%TOMCAT_HOME%/conf/web.xml文件中把虚拟路径显示目录给禁止啦，此时可以在tomcat的web.xml文件中找到：

     <servlet>
        <servlet-name>default</servlet-name>
        <servlet-class>org.apache.catalina.servlets.DefaultServlet</servlet-class>
        <init-param>
            <param-name>debug</param-name>
            <param-value>0</param-value>
        </init-param>
        <init-param>
            <param-name>listings</param-name>
            <param-value>false</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
将listings的value改为true,然后重新启动tomcat，在输入http://localhost:8080/Urban/login.jsp，测试成功！
如果项目开发完成，准备部署在服务器上时，记住要把web.xml文件中参数listings的值改为false，这样可以避免把项目的部署路径呈现给使用者！