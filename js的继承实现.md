---
title: js的原型及继承实现
date: 2016-05-10 14:31:57
tags: js
---
# prototype
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
    


  [1]: ./images/Image%201.png "Image 1.png"