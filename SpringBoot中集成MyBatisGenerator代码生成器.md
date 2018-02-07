---
title: SpringBoot集成MyBatis Generator代码生成器
date: 2018-02-07 16:44:43
tags: [Spring Boot, Mybatis ]
---
在原有SpringBoot+gradle构建的项目上（项目地址[chen1218chen/Springboot-Mybatis-Gradle][1]）继续集成MyBatis Generator来自动生成model层、dao层代码，以实现快速开发。

## 修改build.gradle文件
首先，在gradle的配置文件中添加依赖：

    compile group: 'org.mybatis.generator', name: 'mybatis-generator-core', version:'1.3.2'

## generator.xml
在src/main/resource下添加generator.xml配置文件，根据此文件中的配置生成对应需要的代码，如下：

    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE generatorConfiguration PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
    		"http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
    <generatorConfiguration>
     
    	<context id="DB2Tables" targetRuntime="MyBatis3">
     
    		<!--自动实现Serializable接口-->
    		<plugin type="org.mybatis.generator.plugins.SerializablePlugin"></plugin>
     
    		<!-- 去除自动生成的注释 -->
    		<commentGenerator>
    			<property name="suppressAllComments" value="true" />
    		</commentGenerator>
     
    		<!--数据库基本信息-->
    		<jdbcConnection driverClass="com.mysql.jdbc.Driver"
    						connectionURL="jdbc:mysql://127.0.0.1:3306/boot?characterEncoding=UTF-8"
    						userId="myuser"
    						password="123456">
    		</jdbcConnection>
     
    		<!--生成实体类的位置以及包的名字-->
    		<javaModelGenerator targetPackage="com.cc.entity"
    							targetProject="src\main\java">
    			<property name="enableSubPackages" value="true" />
    			<property name="trimStrings" value="true" />
    		</javaModelGenerator>
     
     		<!--生成映射文件存放位置-->
            <sqlMapGenerator targetPackage="com.cc.dao" targetProject="src\main\java">
                <property name="enableSubPackages" value="true"/>
            </sqlMapGenerator>
            <!--生成Dao类存放位置,mapper接口生成的位置-->
            <javaClientGenerator type="XMLMAPPER" targetPackage="com.cc.dao" targetProject="src\main\java">
                <property name="enableSubPackages" value="true"/>
            </javaClientGenerator>
            <!--生成对应表及类名-->
            <table tableName="item"  domainObjectName="Item"></table>
     		<table tableName="image"  domainObjectName="Image"></table>
    		
    		<!--对应的表名，以及实体名-->
    		<!-- <table tableName="t_emp" domainObjectName="EMP" ></table> -->
     
    	</context>
     
    </generatorConfiguration>

> 需要注意的是，需提前在数据库中建好对应的表，否则会报错`Table configuration with catalog null, schema null`


## 执行类
最后，在写一个main函数来启动运行。

    package com.cc.main;

    import org.mybatis.generator.api.ShellRunner;

    /**
     * 执行类
     */
    public class MybatisGeneratorApp
    {
        public static void main( String[] args )
        {
            args = new String[] { "-configfile", "src\\main\\resources\\generator.xml", "-overwrite" };
            ShellRunner.main(args);
        }
    }

这样刷新一下工程即可看到新生成的java文件，如下图所示：

![enter description here][2]


  [1]: https://github.com/chen1218chen/Springboot-Mybatis-Gradle
  [2]: ./images/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20180207165700.png "微信截图_20180207165700.png"