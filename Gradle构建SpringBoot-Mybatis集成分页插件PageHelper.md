---
title: Gradle构建SpringBoot+Mybatis集成分页插件PageHelper
date: 2017-08-11 13:38:52
tags: [Spring Boot,Gradle,Mybatis,PageHelper]
---
可点击此处下载完整项目哦：[Springboot-Mybatis-Gradle][1]

使用Gradle构建SpringBoot项目，使用Mybatis来持久化，尝试集成PageHelper分页插件，依然顺利都令到我吃惊，so easy!
首先，build.gradle中配置依赖

    dependencies {
        compile group: 'com.github.pagehelper', name: 'pagehelper', version: '4.1.0'
    }
其次，写一个注册类

    package com.cc.config;

    import java.util.Properties;  
    import org.springframework.context.annotation.Bean;  
    import org.springframework.context.annotation.Configuration;  
    import com.github.pagehelper.PageHelper;  

    /*  
    * 注册MyBatis分页插件PageHelper  
    */  

    @Configuration  
    public class MybatisConf {  
          @Bean  
          public PageHelper pageHelper() {  
             System.out.println("=========MyBatisConfiguration.pageHelper()");  
              PageHelper pageHelper = new PageHelper();  
              Properties p = new Properties();  
              p.setProperty("offsetAsPageNum", "true");  
              p.setProperty("rowBoundsWithCount", "true");  
              p.setProperty("reasonable", "true");  
              pageHelper.setProperties(p);  
              return pageHelper;  
          }  
    } 
最后，在controller中直接使用即可：  

      @ApiOperation(value="根据姓名查找用户", notes="根据name来查找")
    @ApiImplicitParam(paramType="query", name = "name", value = "用户姓名", required = true, dataType = "String")
    @RequestMapping(value="/findByName",method=RequestMethod.POST) 
    @ResponseBody
    public List<User> findByName(@RequestParam(value="name", required=true) String name){  
        /*  
         * 第一个参数是第几页；第二个参数是每页显示条数。  
         */  
        PageHelper.startPage(1,2);  
        return userService.fingByName(name);  
    }
深入使用还待我继续研究![enter description here][2]


  [1]: https://github.com/chen1218chen/Springboot-Mybatis-Gradle
  [2]: ./images/1502430421592.jpg "1502430421592.jpg"