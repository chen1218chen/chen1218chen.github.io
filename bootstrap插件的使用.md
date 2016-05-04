---
title: bootstrap 常用插件使用
tags: bootstrap
---

[TOC]

# select2插件的使用 #
 
> 选择select2插件的使用

    $(id).select2({
					data : data,
					minimumResultsForSearch : Infinity,// 禁用搜索
					allowClear : true
				});

# bootstrap-validator表单验证 #
>使用bootstrap-validator插件
>
>reset重置表单

    $('#btn-reset').click(function() {
		$('#changeForm').data('bootstrapValidator').resetForm(true);
	});

#  bootstrap-dialog #
## 1. 页面加载

    function changePasswd(){
     BootstrapDialog.show({
    	 title : "修改密码",
         message: function(dialog) {
             var $message = $('<div></div>');
             var pageToLoad = dialog.getData('pageToLoad');
             $message.load(pageToLoad);
             return $message;
         },
         data: {
             'pageToLoad': 'changePasswd.html'
         },
        	buttons: [{
             id: 'btn-ok',   
             icon: 'glyphicon glyphicon-check',       
             label: 'OK',
             cssClass: 'btn-success', 
             autospin: false,
             action: function(dialogItself){    
            	 change();
          	     dialogItself.close();
             }
         }, {
        	 id:'btn-reset',
             cssClass: 'btn-warning',
             label: '重置',
             icon: 'glyphicon glyphicon-repeat',
             action : function(){
            	 document.getElementById("changeForm").reset();
            	$('#btn-reset').click(function() {
            	});
             }
         }, {
        	 id:'btn-close',
        	 icon:'glyphicon glyphicon-remove',
             label: 'Close',
             cssClass: 'btn-danger',
             action: function(dialogItself){
                 dialogItself.close();
             }
         }]
     });
    }
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
    
# bootstrap Table #

