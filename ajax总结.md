---
title: ajax总结
date: 2016-04-22 09:27:06
tags: [ajax, JavaScript]
---
[TOC]

# ajax总结
## 参数

    $.ajax({
    	url : "delTerminalAction.action/config.json",
    	type : "post/get",
    	async : false,//表示同步
    	traditional : true,// 这样就能正常发送数组参数了
    	dataType : "json/xml/text/html",
    	contentType: "application/x-www-form-urlencoded; charset=utf-8",//设置编码
    	data : {
    		"ids" : ids,
    	},	
    	success : function(data) {
    		$.each(data , funtion(i, item){
    			alert(item[i]);
    			//或者是
    			alert(item.tagName);
    		});
    	},
    	error : function() {
    		alert("删除失败！！");
    	}
    });
##  $.ajaxSetup()
jQuery的$.ajaxSetup方法可以设置AJAX请求的全局默认参数选项，当程序中需要发起多个AJAX请求时，则不用再为每一个请求配置请求的参数。
既然是全局默认选项，所以，该项中设置的参数会影响所有的$.ajax请求，如$.get()，$.post()，$.ajax等

    $.ajaxSetup({  
        contentType : 'application/json'  
    }); 
## 扩展
get了一个新技能，mark一下。

    options = {
        type : "get/post",
        datatype:"json",
        success : function(){
            alert("success");
        }
    }
    $.ajax($.extend(options,{
        url : "rest/user/queryAll",
        data : {
            "name":"chenchen"
        }
    }))
## $.getJSON

    // $.getJSON相当于$.ajax方法中dataType值为json时的简化
    $.getJSON(url, function(data){
        ...
    })

## $.get

    //可获取int型非json数据
    $.get(url,function(){

    })
原文不定时更新，具体详情请查看[ajax总结][1]


  [1]: http://chen1218chen.github.io/2016/04/22/ajax%E6%80%BB%E7%BB%93/