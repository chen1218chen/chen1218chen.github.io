---
title: JS使用技巧
tags: JS
---

[TOC]

## 函数定义
```
	var func = function(name){
		console.log("终端输出"+name);
	}
```
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