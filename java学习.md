---
title: java
tags: java
---

[TOC]

# java
## 集合
- set
    - HashSet
    - TreeSet
- list
    - ArrayList
    - LinkedList
- map
    - HashMap
    - TreeMap
    
**Collection:** 集合接口，包含Set，List
**java.util.Collections:**  是一个包装类。它包含有各种有关集合操作的静态多态方法。此类不能实例化，就像一个工具类，服务于Java的Collection框架。

## 构造函数
1. 必须与类名同名
2. 可以有多个或者0个
3. 没有返回值
4. 伴随着new操作一起被调用
```
public class A{
   public A(){
      System.out.println("调用了无参的构造函数");
   }
   public A(String mess){
      System.out.println("调用了有参的构造函数\n"+
         "参数内容为："+mess);
   }
}

public class Test{  
   public static void main(String [] args){  
       A a_1=new A();//调用无参的构造函数  
       A a_2=new A("Hello");//调用有参的构造函数  
   }  
}
```
