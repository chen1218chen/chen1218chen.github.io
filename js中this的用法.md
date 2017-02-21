---
title: js中this的用法
date: 2016-09-23 10:18:23
tags: JavaScript
---
[TOC]

即使网上有很多关于js中this的用法详解，我还是想写一篇属于自己的理解，以方便自己日后回顾。
常见的用法分为以下四种：
## 代表全局变量

    var x = 0;
    function test(){
    	this.x = 1;
    }

    test();
    alert(x); // 1
    
## 作为构造函数
这里所说的构造函数是指用函数new一个新对象,即：

    var t = new test();
这时，this指这个新对象

    var x = 2;
    function test(){
    	this.x = 1;
    }

    var t = new test();
    alert(t.x); // 1
    alert(x);   // 2
    
## 作为对象的方法调用
这时this值上级对象

    function test(){
    	alert(this.x); // 2
    	this.x = 1;
    	alert(this.x); // 1
    }

    var t = {};
    t.x = 2;
    t.o = test;
    t.o();
## apply
apply是函数对象的一个方法，作用是改变函数的调用对象，它的第一个参数就是改变后调用这个函数的对象，this就指的是这个对象。如果参数为空，默认调用全局对象

    var x = 3
    function test(){
    	alert(this.x);
    }

    var t = {};
    t.x = 2;
    t.o = test;
    t.o(); //2
    t.o.apply(); //3
    t.o.apply(t); //2 相当于t.o();

apply的详解查看[js的原型及继承实现][1]一文


  [1]: http://chen1218chen.github.io/2016/05/10/js%E7%9A%84%E7%BB%A7%E6%89%BF%E5%AE%9E%E7%8E%B0/