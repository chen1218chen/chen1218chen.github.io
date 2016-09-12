---
title: bootstrap-dialog使用
date: 2016-09-09 17:05:23
tags: [Bootstrap, bootstrap-dialog]
---
[TOC]

## 页面加载

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

