---
title: Spring集成Log4j
date: 2016-09-18 15:50:43
tags: [Spring, Log4j]
---
Spring项目中集成Log4j 
1. 下载好log4j的jar包（例如：log4j-1.2.11.jar）
2. web.xml


    <!--log4j配置文件加载-->  
    <context-param>      
       <param-name>log4jConfigLocation</param-name>      
       <param-value>/WEB-INF/log4j.properties</param-value>      
    </context-param>  
    <!--启动一个watchdog线程每1800秒扫描一下log4j配置文件的变化-->  
    <context-param>      
       <param-name>log4jRefreshInterval</param-name>      
       <param-value>1800000</param-value>      
    </context-param>   

    <!--spring log4j监听器-->  
    <listener>      
       <listener-class>org.springframework.web.util.Log4jConfigListener</listener-class>      
    </listener> 
只配置listener也可以
3. log4j.properties
日志的主配置文件


    log4j.rootLogger=info,stdout,debug,error    
    log4j.logger.org.springframework=info
    #log4j.logger.org.springframework.web=debug
    log4j.appender.stdout=org.apache.log4j.ConsoleAppender    
    log4j.appender.stdout.layout=org.apache.log4j.PatternLayout    
    log4j.appender.stdout.layout.ConversionPattern=[%-5p] [%d{HH\:mm\:ss}] %c - %m%n    

    log4j.logger.info=info    
    log4j.appender.info=org.apache.log4j.DailyRollingFileAppender    
    log4j.appender.info.layout=org.apache.log4j.PatternLayout    
    log4j.appender.info.layout.ConversionPattern=[%-5p] [%d{HH\:mm\:ss}] %c - %m%n    
    log4j.appender.info.datePattern='.'yyyy-MM-dd    
    log4j.appender.info.Threshold = INFO    
    log4j.appender.info.append=true    
    log4j.appender.info.File=${catalina.home}/logs/log4j/info.log
    log4j.appender.warn.File=${catalina.home}/logs/log4j/warn.log    

    log4j.logger.debug=debug    
    log4j.appender.debug=org.apache.log4j.DailyRollingFileAppender    
    log4j.appender.debug.layout=org.apache.log4j.PatternLayout    
    log4j.appender.debug.layout.ConversionPattern=[%-5p] [%d{HH\:mm\:ss}] %c - %m%n    
    log4j.appender.debug.datePattern='.'yyyy-MM-dd    
    log4j.appender.debug.Threshold = DEBUG    
    log4j.appender.debug.append=true    
    log4j.appender.debug.File=${catalina.home}/logs/log4j/debug.log

    log4j.logger.warn=warn    
    log4j.appender.warn=org.apache.log4j.DailyRollingFileAppender    
    log4j.appender.warn.layout=org.apache.log4j.PatternLayout    
    log4j.appender.warn.layout.ConversionPattern=[%-5p] [%d{HH\:mm\:ss}] %c - %m%n    
    log4j.appender.warn.datePattern='.'yyyy-MM-dd    
    log4j.appender.warn.Threshold = DEBUG    
    log4j.appender.warn.append=true    

    log4j.logger.error=error    
    log4j.appender.error=org.apache.log4j.DailyRollingFileAppender    
    log4j.appender.error.layout=org.apache.log4j.PatternLayout    
    log4j.appender.error.layout.ConversionPattern=[%-5p] [%d{HH\:mm\:ss}] %c - %m%n    
    log4j.appender.error.datePattern='.'yyyy-MM-dd    
    log4j.appender.error.Threshold = ERROR    
    log4j.appender.error.append=true    
    log4j.appender.error.File=${catalina.home}/logs/log4j/error.log

4. 代码中写入日


    private static final Logger logger = LoggerFactory.getLogger(XXX.class);
    logger.info("XXXX");
    logger.warn("XXXX");
    logger.error("XXXX");