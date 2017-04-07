---
title: ES6新知识get
date: 2017-04-07 14:44:09
tags: JavaScript
---
[TOC]

ECMAScript6 作为新的Javascript标准，与ECMAScript5有着很大区别，项目实践中积累新知识。
## const,var,le区别
- const定义的变量不可以修改，而且必须初始化。常用来定义常量。类似于java中常定义的Constants常量。
- var定义的变量可以修改，如果不初始化会输出undefined，不会报错。
- let是块级作用域，函数内部使用let定义后，对函数外部无影响。


    let c = 3;
    console.log('函数外let定义c：' + c);//输出c=3
    function change(){
    let c = 6;
    console.log('函数内let定义c：' + c);//输出c=6
    } 
    change();
    console.log('函数调用后let定义c不受函数内部定义影响：' + c);//输出c=3

## 箭头函数
ES6出来之后，箭头函数频繁出现啊，经常看的我云里雾里的，狠下决心好好弄清楚这个到底是什么鬼，认真看看其实也就是匿名函数的简化表达而已。比如，


    var str = a =>a + a;
相当于：

    var str = function(a) {
        return a+a;
    }
虽然理解了，还是很不习惯啊。。。