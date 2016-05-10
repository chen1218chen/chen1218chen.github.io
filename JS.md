---
title: JS使用技巧
tags: JS
---

[TOC]
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
    c2.plus();//
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

## JS中this关键字
1. this指向全局变量


    var a=1;
    console.log(this);// Window全局对象
    console.log(this.a);//1
2. 函数作用域中的this


    var a=1;
    function test(){
    	var a=3;
    	console.log(this.a);
    }
    test.a=5;
    test();//1
可看出this并没有指向函数作用域，也并未指向函数本身，this最终指向的都是全局对象
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