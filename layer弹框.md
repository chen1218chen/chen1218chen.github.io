---
title: layer弹框
date: 2018-04-10 15:53:27
tags:
---

## layer.prompt() 确认框


    layer.prompt({
    	title: '是否发送地址数据到'+row.jurisdiction+'?',
    	value: row.address,
    	formType: 2
    }, function(text, index){
        layer.close(index);
        console.dir(text);
        $.ajax({
        	url:'rest/app/sendItem',
        	type:"post",
        	data:{
        		id:row.id
        	},
        	success:function(){
        		layer.msg("发送成功："+text);	
        		back.refresh();
        	},
        	error:function(){
        		layer.msg("发送失败："+text);	
        	}
        })
        
      });