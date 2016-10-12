---
title: JS基础概念之大杂烩
tags: JS
---

[TOC]

## 执行环境和执行环境对象
执行环境由函数在执行过程中发生的所有事物组成，所有函数中定义的变量和函数都是执行环境的一部分。**变量在作用域内，即函数运行时变量在当前执行环境中，可访问**。
属于执行环境的变量和函数，被保存在执行环境对象中。执行环境对象是对执行环境的ECMA标准的实现。

![enter description here][1]
执行环境是Javascript引擎的一部分，在Javascript中不能直接访问。
## js对象和原型链
**javascript的对象是基于原型的，java对象是基于类的。**

![enter description here][2]

![enter description here][3]

![enter description here][4]
每个方法中，首先创建了对象的模板。模板在基于类的编程中叫做**类**，在基于原型的编程中叫做**原型对象**，作用都是为了用来：作为创建对象的结构。

创建构造对象，基于类的编程中构造对象在类的内部定义；在js中对象的构造函数和原型是分开设置的，需要用表2-3中的红框部分`Prisoner.prototype = proto;`将它们连接在一起。

最后，实例化对象。
## 匿名函数
1. 用一个局部变量来保存匿名函数


    var prison = function(){
        ...
    }
2. 自执行匿名函数
jquery的实现就是使用了自执行的匿名函数，


    (function(){
        ...
    })();
    
![enter description here][5]
> 传参


    (function($){
        console.log($); //jQuery
        ...
    })(jQuery)
> 可以将自执行函数保存在一个变量中，如下：


    var worker = (function(){
        var person_name = "cc";
        return {
            name: person_name
        }
    })();
    console.log(worker.name); // cc
自执行匿名函数被用来控制变量的作用域，阻止变量泄露到代码的其他地方去。常用于创建Javascript插件，不会和应用代码起冲突，因为它不会向全局名字空间添加任何变量。

## setTimeOut()及js引擎、线程
js引擎是单线程的，基于事件驱动的语言，执行顺序遵循一个事件队列的机制。js引擎处理到与其他线程相关的代码，就会分发给其他线程，他们处理完后需要JS引擎处理是，在事件队列后面添加一个任务。

    <script>
        alert("1");
        setTimeOut("alert(2)",0);
        alert(3);
    </script>
> 执行顺序1,3,2

可看出setTimeOut(fn,0);不是立即执行的。原因是：当程序执行到setTimeOut这里，会开启一个定时器线程，然后继续执行新的代码，该线程会在指定的时间后在事件队列里面插入一个新任务，所以setTimeOut中的操作会在所有主任务之后，执行顺序就是1,3,2了。
**setTimeOut新解：在指定时间内，将任务放入事件队列，等待js引擎空闲时被执行**
js引擎和GUI引擎（渲染引擎）是互斥的。一个挂起一个执行。
浏览器引擎是多线程的，它们在内核的控制下保持同步，一个浏览器至少实现三个常驻线程。：js引擎线程、GUI引擎线程、浏览器时间触发线程。

## 闭包
js采用词法作用域，即函数的执行依赖于变量的作用域，而变量的作用域是在函数定义是决定的，对应一个作用域链。函数对象可以通过作用域链相互连接起来，函数体的内部变量都保存在函数作用域内，这种特性在计算机中成为闭包。从技术的角度上讲，函数就是一个闭包。

    function test() {
    	var a = 4;
    	return {
    		add : function() {
    			a++;
    			console.log(a);
    		},
    		plus : function() {
    			a--;
    			console.log(a);
    		}
    	};
    }
    var c1 = new test();
    var c2 = new test();
    c1.add();//5
    c2.plus();//3
两个变量不会相互影响。
初始化私有变量值

    function test(n){
    	return{
    		get count(){
    			return n++;
    		},
    		set count(m){
    			if(n>m)
    				n=m;
    			else throw Error("ddd");
    		}
    	};
    }
    var t = test(100);
    t.count;
    console.log(t.count);//100
    t.count=99;
    console.log(t.count);//99

3. 对象的方法调用


    var a=1;
    function test(){
    	var a=3;
    	console.log(this.a);
    }
    var obj={
    		a:6,
    		fn:test
    };
    obj.fn();//6
    obj.fn.call();//1
this指向当前调用该方法的对象，使用对象的方法调用call，this默认指向的是全局对象，若想改变this指向

    var a=1;
    function test3(){
    	console.log(this.a);
    }
    test3.call();//1
    var obj={
    	a:8,
    	fn:test3
    }
    var para={a:9};
    obj.fn.call(para);//9
call和apply都是Function对象的方法
4. 构造函数


    function test2(){
    	this.a=7;
    }
    var o = new test2();
    console.log(o); //7
5. 嵌套函数的作用域


    var a=1;
    function test4(){
        console.log(this.a);//2
        function test5(){
            console.dir(this.a);//1
        }
        test5();
    }
    var obj = {a:2,fn:test4};
    obj.fn();

## 函数定义
```
	var func = function(name){
		console.log("终端输出"+name);
	}
```
### 嵌套函数
嵌套函数定义，test2只能在test中调用。不能再其他函数中调用

    function test(){
        var a=1;
        function test2(){
            a++;
            console.log(a);
        }
        test2();
    }
    test();//2

**console.log();**终端打印输出，最早是火狐在fiebug中用来调试
##  prompt
```
var  username = prompt("what is your name?");
//username则可以获取到输入的参数
```
## js中引用jsp代码
在**' '**中直接写JSP代码
```
function getCId(){
	return '<%=session.getAttribute("cId")%>';
}
``` 


## JSON总结
```
//str->obj
JSON.parse();
//obj->str
JSON.stringify();
```

## js字符串长度限制
<a href>是使用get传递参数，url无论如何都有2k的长度限制


  [1]: ./images/Image%202.png "Image 2.png"
  [2]: ./images/Image%203.png "Image 3.png"
  [3]: ./images/Image%204.png "Image 4.png"
  [4]: ./images/Image%205.png "Image 5.png"
  [5]: ./images/Image%206.png "Image 6.png"