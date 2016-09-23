---
title: JS技术整理
tags: JS
---

[TOC]
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