---
title: js的继承实现
date: 2016-05-10 14:31:57
tags: js
---
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
    