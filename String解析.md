---
title: String解析
date: 2016-08-04 10:22:34
tags: java
---
[TOC]

String类型是一个特殊的包装类，不属于8种的基本类型之一，对象的默认值是null，所以String的默认值也是null。
## 存放区域
字符串是一个特殊包装类,其引用是存放在栈里的,而对象内容必须根据创建方式不同定(常量池和堆).有的是编译期就已经创建好，存放在字符串常 量池中，而有的是运行时才被创建.使用new关键字，存放在堆中。
## 比较
比较数值是否相等时，用'equals()'方法；比较引用值是否指向同一个对象时，用==。 

这个对于String简单来说就是比较两字符串的Unicode序列是否相当，如果相等返回true;而==是 比较两字符串的地址是否相同，也就是是否是同一个字符串的引用。
new String()和new String(" ")都是申明一个新的空字符串，是空串不是null。
## 两种不同初始化
    String str = new String("aaa");
    String str1 = "aaa";

### String str = new String("aaa");

    String str1 = "abc";   
    String str2 = "abc";   
    System.out.println(str1==str2); //true  
可以看出str1,str2指向同一个对象。
以上代码创建了一个对象，"abc"存储于字符串常量池中。两个引用str1和str2，指向"abc"。
### String str1 = "aaa";

    String str1 =new String ("abc");   
    String str2 =new String ("abc");   
    System.out.println(str1==str2); // false  
用new的方式生成不同的对象。
以上代码创建了三个对象，'abc'存储于字符串常量池中，两个new String()对象位于堆中，栈中两个指向堆的引用str1,str2
所以第二种方法有利于节省内存空间，提高运行速度。
## StringBuffer
StringBuffer：可变长度
String：不可变长度