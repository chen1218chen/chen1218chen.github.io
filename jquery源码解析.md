---
title: jquery源码解析
date: 2016-05-19 10:55:30
tags: jQuery
---


## (function(a,b){})()
JQuery源码的开头

    (function(a,b){
        
    })()
通常(function(){})()用来封装一些私有成员或者公共成员的导出。
js中函数就是一个Function对象，匿名函数指没有函数名的函数，即：

    function(x,y){
        return x+y;
    }
这样就存在一个匿名函数的引用问题，通过一下方法引用：
方法一：

    var abc = function(x,y){return x+y};
    abc(1,2); //3
方法二：

    (function(x,y){
        return x+y;
    })(1,2); //3
方法一是我们常用的方法，方法二中，小括号的作用是把代码分块，而且每一块会有一个返回值，值既是小括号中表达式的返回值，即如同函数名作用一般取得引用位置，再后面加上()参数列表，实现普通函数的调用。
()运算符将一个表达式包裹起来作为一个整体进行运算，然后返回这个整体的值。   
那么上面的(function(){})()中左侧定义function的()也是这个作用，将这个function给包裹起来，然后返回这个function。我们调用方法一般是a();那么(function(){})的作用就是返回这个function对象，然后(function(){})()右侧的()表示调用这个function

    var abc = function(x,y){return x+y};
中abc的constructor与匿名函数的constructor一样，即，两者的实现一样。
## jQuery中如何实现$引用
JS中window对象是全局变量，所有浏览器都支持window对象。它表示浏览器窗口。

     window.jQuery = window.$ = jQuery;
所有 JavaScript 全局对象、函数以及变量均自动成为 window 对象的成员。
全局变量是 window 对象的属性。
全局函数是 window 对象的方法。

## Sizzle引擎
jQuery内部使用Sizzle引擎，处理各种选择器
## 闭包
简单来说是指允许程序中调用函数中的局部变量。
## class
简单来说可以把jQuery看成一个类，在原型上绑定方法就相当于成员方法，在jQuery上绑定方法，相当于类的静态方法。

    jQuery.A = function (){}
    jQuery.prototype.B = function(){}
相当于：
    
    class jQuery(){
        public static A(){};
        public B(){};
    }
## $.extend、$.fn.extend   
jQuery为开发插件提供了两个方法

    //添加类方法，为扩展jQuery类本身，为自身添加新的方法。
    jQuery.extend();
    jQuery.fn.extend();//添加成员函数，给jQuery对象添加方法

    //即指向jQuery对象的原型链，对其它进行的扩展，作用在jQuery对象上面
    jQuery.fn=jQuery.prototype={
        //...
    }
 ![enter description here][1]
## 别名

    var $j=JQuery.noConflict(); 
    $j('#msg').hide();//此处$j就代表JQuery 
## jQuery效率
1. ID选择器的效率最高，直接使用了浏览器的内置函数document.getElementById(),效率很高。
2. 少直接用class选择器
class选择器是效率最慢的
3. 多用父子关系，少用嵌套关系。
4. 缓存jQuery对象
5. 每一个$()的调用都会导致一次新的查找，所以，采用链式调用和设置变量缓存结果集，减少查找，提升性能。


  [1]: ./images/Image%201.png "Image 1.png"