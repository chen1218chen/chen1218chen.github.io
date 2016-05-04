---
title: java
tags: java
---

[TOC]

# java
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