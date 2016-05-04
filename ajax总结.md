---
title: ajax总结
date: 2016-04-22 09:27:06
tags: ajax，js
---
## ajax总结

	$.ajax({
		url : "delTerminalAction.action/config.json",
		type : "post/get",
		async : false,
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
	// $.getJSON相当于$.ajax方法中dataType值为json时的简化
	$.getJSON(url, function(data){
	
	})
	//可获取int型非json数据
	$.get(url,function(){

	})
