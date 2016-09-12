---
title: jQuery-confirm
date: 2016-09-09 17:08:00
tags: [Bootstrap, jQuery]
---

[TOC]

# Jquery-confirm
## 1. alert使用 ##
>使用 Jquery-confirm.js插件

    function alertView(str){
    	$.alert({
    		animation : 'zoom',
    		animationBounce : 2,
    		keyboardEnabled : true,
    		title : false,
    		content : str,
    		theme : 'white',
    	});
    }

## 2. confirm使用 ##
>使用 Jquery-confirm.js插件

    $.confirm({
    	title: '退出系统登陆吗？', 
    	content : false,
    	// autoClose: 'confirm|10000',
    	confirmButtonClass : 'btn-info',
    	cancelButtonClass : 'btn-danger',
    	confirm : function() {
    		$.ajax({
    			type : "post",
    			url : "logoutAction.action",
    			async: false,
    			success :function(){
    				location.href = "../login.jsp";
    			}
    		});
    	},
    	cancel : function() {
    		alert('canceled');
    	}
    });