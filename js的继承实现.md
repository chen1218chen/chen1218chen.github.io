---
title: js的原型及继承实现
date: 2016-05-10 14:31:57
tags: js
---
[TOC]

# prototype继承
所有通过new Object()创建的对象都其对应的原型对象，且通过Object.prototype获得对原型对象的引用。所以，通过new和构造函数调用创建对象的原型即构造函数的prototype的值。
- Object.prototype没有原型
- 所有的内置构造函数都有一个继承自Object.prototype的原型
- 原型对象是类的唯一标识
- 每个JS函数都自动拥有一个prototype属性，属性的值是一个对象。对象包含一个constructor属性，此属性值是一个对象如下图：


        function People(props){
            this.name = props.name || 'unnamed';
        }
        
        People.prototype.hello = function(){
            alert('hello,'+this.name);
        }
        
![enter description here][1]
## constructor

    var p = new F();
    p === F.prototype;  //true
    var c = p.constructor;
    c === F;  // true
由上可以看出，


    var o = new F();
    o.constructor === F; //true,constructor属性指代这个类
A的实例继承自A.prototype，B的实例继承自B.prototype。所以B继承A，即B继承A.prototype。


        var object = new Object();
        object == Object.prototype
Object.prototype即是一个新的对象
**（一般情况下Object泛指一个JS对象，而非真正的Object对象）**
## 原型链
`var date = new Date();` date对象同时继承Date.prototype和Object.prototype，这一系列链接的原型对象就是原型链。
# 继承
## 类型检测
1. instanceof
```
    a instanceof A //如果a继承自A，则返回true
```
可以不是直接继承
2.  constructor属性检测


    function typeValue(x){
    	if(x==null) return ;
    	switch(x.constructor){
    	case Number: return "Number"+x;
    	case String: return "String"+x;
    	case Date: return "Date"+x;
    	...
    	
    	}
    }
JS继承的实现，通过中间函数F()

    function inherits(child,parent){
        var F = function(){};
        F.prototype = parent.prototype;
        child.prototype = F;
        //修复构造函数，否则会调用F的constructor
        child.prototype.constructor = child;
    }

    function People(props){
        this.name = props.name || 'unnamed';
    }

    People.prototype.hello = function(){
        alert('hello,'+this.name);
    }

    function Student(props){
        People.call(this,props);
        this.age = props.age||1;
    }
    inherits(Student,People);
    var cc = new Student();
原型链的继承顺序是：

    cc--->Student.prototype--->People.prototype--->Object.prototype
原型链对了，继承顺序就对了。

# apply(),call()继承
call和apply是为了动态改变this而出现的。
第一个函数是调用函数的母对象。它是调用上下文，函数体内通过this来获得对它的引用。可以改变this的指示方向。
比如，以对象o的方法的形式调用函数f()，并传入两个实参，如下：

    f.call(o,1,2);
    f.apply(o,[1,2]);  
apply与call的区别仅在于，apply的第二个参数可以是数组形式，而且不必一一指出参数。如下：
三种不同的函数调用方法：

    obj.myFunc();
    myFunc.call(obj,arg);
    myFunc.apply(obj,[arg1,arg2..]);
## apply()
apply是函数对象的一个方法，作用是改变函数的调用对象，它的第一个参数就是改变后调用这个函数的对象，this就指的是这个对象。如果参数为空，默认调用全局对象

    Function.apply(obj,args);  //方法能接收两个参数
obj：这个对象将代替Function类里this对象
args：这个是数组，它将作为参数传给Function（args-->arguments）
如下例子所示：

    function Person(name,age){
    	this.name = name;
    	this.age = age;
    	this.hello = function(){
    		console.log("hello!"+this.name);
    	}
    }

    function Student(name, age, school){
        this.name = name;
        this.age = age;
        this.school = school;
    }
     var p = new Person("person",100);
     var s = new Student("cc",25,"85");
     p.hello.apply(s);//hello!cc
     p.hello.call(s); //hello!cc
student对象没有hello方法，如果想用就可以通过apply来调用。

  [1]: ./images/Image%201.png "Image 1.png"