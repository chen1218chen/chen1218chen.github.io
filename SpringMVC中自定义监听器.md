---
title: SpringMVC中自定义监听器
date: 2017-10-09 16:32:40
tags: [springMVC, Listener]
---

最近项目需求，需要在SpringMVC中使用Java监听器，并且需要注入service层，@Resource 没有任何效果，一直报空，后来找到问题所在，spring容器的初始化也是由Listener（ContextLoaderListener）完成的，在调用自定义的listener前要确保，spring已经初始化完毕，所以需要需在web.xml中先配置初始化spring容器的Listener（ContextLoaderListener），然后在配置自己的Listener。


    public class ConfigListener implements ServletContextListener {  
       
        @Override public void contextInitialized(ServletContextEvent sce) {     
            ConfigService configService = WebApplicationContextUtils.getWebApplicationContext(sce.getServletContext()).getBean(ConfigService.class);  
            configService.initConfig();  
        }  

        @Override public void contextDestroyed(ServletContextEvent sce) {  
    }  
如上，ConfigService是要在listener中使用的bean。



参考资料：
[在自定义Listener中使用Spring容器管理的bean][1]
[如何在自定义Listener（监听器）中使用Spring容器管理的bean][2]


  [1]: http://blog.csdn.net/qq_21899803/article/details/52738426
  [2]: http://www.cnblogs.com/fjdingsd/p/5731982.html