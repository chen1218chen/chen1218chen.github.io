---
title: es6之promise
date: 2018-05-07 13:30:54
tags: [ES6, Promise]
---

ES6中关于Promise回调的小示例，以及resolve，reject的用法和catch的使用

    back.removeTaskList = function(ids){
    	return new Promise((resolve, reject) => {
    		$.ajax({
    			type:"post",
    			url:"rest/task/removeTask",
    			traditional : true,
    			data:{
    				ids:ids
    			},
    			success:function(data){
    				resolve(data);
    			},
    			error:function(data){
    				reject("数据正在使用中，删除失败!");
    			}
    		});
    	})
    }

    back.removeTaskList(ids).then((data)=>{
    	if(data){
    		layer.msg("删除成功！");
    	}else{
    	    layer.msg("删除失败!");
        }
    }).catch(function(data){
        layer.msg(data);
    });