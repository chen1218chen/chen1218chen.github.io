---
title: No Dialect mapping for JDBC type: -1
date: 2017-12-27 09:49:52
tags: Hibernate
---

hibernate中使用sql语句进行查询，后台报错
`No Dialect mapping for JDBC type: -1; nested exception is org.hibernate.MappingException: No Dialect mapping for JDBC type: -1`
查询了一下说是查询的数据中使用了hibernate不支持的数据类型，就是说hibernate没有注册该类型，查看`java.sql.Types`维护的一些常量根据报错的数据来查看是哪个类型出错，-1即`LONGVARCHAR`类型出错。

![enter description here][1]

## 解决方法1
 写一个Dialect的子类，这里因为我使用MySQL数据库所以 extends MySQL5Dialect类，


    package com.aerors.config;

    import java.sql.Types;

    import org.hibernate.Hibernate;
    import org.hibernate.dialect.MySQL5Dialect;   
    public class DialectForInkfish extends MySQL5Dialect {   
        public DialectForInkfish() {   
            super(); 
            //JDBC type:1  
            registerHibernateType(Types.CHAR, Hibernate.STRING.getName());  
              
            //JDBC type:-9  
            registerHibernateType(Types.NVARCHAR, Hibernate.STRING.getName());  
              
            //JDBC type:-16  
            registerHibernateType(Types.LONGNVARCHAR, Hibernate.STRING.getName());  
              
            //JDBC type:3  
            registerHibernateType(Types.DECIMAL, Hibernate.DOUBLE.getName());  
              
            //JDBC type:-1  
            registerHibernateType(Types.LONGVARCHAR, Hibernate.STRING.getName()); 
        }   
    } 
修改Hibernate配置文件hibernate.cfg.xml，把

    <property name="dialect">org.hibernate.dialect.MySQL5Dialect</property>   
修改为：  

    <property name="dialect">com.aerors.config.DialectForInkfish</property>  
当`JDBC type`为1，-9，-16，3等其他值时，也可以添加对其错误码对应的的数据类型的支持。

## 解决方法二
抛弃sql语句查询，改为HQL语句查询
## 解决方法三
使用CONVERT函数，将text转成varchar，简单方便

    CONVERT(varchar(100), isnull(dateTime, '')) 
dateTime是text类型变量名，isnull函数判断下是否为空,一般在时间类型(datetime,smalldatetime)与字符串类型(nchar,nvarchar,char,varchar)
相互转换的时候才用到。

  [1]: ./images/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20171227095422.png "微信截图_20171227095422.png"