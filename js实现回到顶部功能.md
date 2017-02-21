---
title: js实现回到顶部功能及页面定位
date: 2016-05-13 10:56:03
tags: JavaScript
---
## 回到顶部

    $(function(){
    	$("#toTop").css("display","none");
    	
    	$(window).scroll(function() {
    		var client_height = $(window).height();
    		//实现兼容性
    		var scroll_height = document.documentElement.scrollTop ? document.documentElement.scrollTop : document.body.scrollTop;
    		if(scroll_height > client_height) {
    			$("#toTop").css("display","block");
    		} else {
    			$("#toTop").css("display","none");
    		}
    	});
    	
    	$("#toTop").click(function(){
    		$(document.documentElement).animate({
    			scrollTop: 0
    		},200);
    		$(document.body).animate({
    			scrollTop: 0
    		},200);
    	});
    	
    	//正则验证，浏览器总是否含有iphone|ipod|Android|SymbianOS|Windows Phone字样，来判断浏览器类型
    	if (/iphone|ipod|Android|SymbianOS|Windows Phone/i.test(navigator.userAgent)) {
    		location.href = '/jdwsjb/sy_phoneweb/ipadpdxxy/index.shtml';
    	 }
    });  
    <a id="toTop">回到顶部</a>

$(window)：jQuery中获取浏览器当前窗口。

    $(window).resize(function(){
    });
$(document) ：jQuery中获取当前文档，就是我们所看到的整个网页

    $(document).ready(function(){
    });
## 浏览器类型判断
**navigator.userAgent**
正则验证，浏览器总是否含有iphone|ipod|Android|SymbianOS|Windows Phone字样，来判断浏览器类型

    if (/iphone|ipod|Android|SymbianOS|Windows Phone/i.test(navigator.userAgent)) {
    	location.href = '/jdwsjb/sy_phoneweb/ipadpdxxy/index.shtml';
     }
     
     function userBrowser(){
        var browserName=navigator.userAgent.toLowerCase();
        if(/msie/i.test(browserName) && !/opera/.test(browserName)){
            alert("IE");
            return ;
        }else if(/firefox/i.test(browserName)){
            alert("Firefox");
            return ;
        }else if(/chrome/i.test(browserName) && /webkit/i.test(browserName) && /mozilla/i.test(browserName)){
            alert("Chrome");
            return ;
        }else if(/opera/i.test(browserName)){
            alert("Opera");
            return ;
        }else if(/webkit/i.test(browserName) &&!(/chrome/i.test(browserName) && /webkit/i.test(browserName) && /mozilla/i.test(browserName))){
            alert("Safari");
            return ;
        }else{
            alert("unKnow");
        }
    }

## 滚动条纵坐标
html5中获取当前页面的滚动条纵坐标位置:**document.documentElement.scrollTop**
在w3c标准下document.body.scrollTop恒为0，且IE5.5之后不再支持document.body.scrollX属性了。所以需要加上以下判断

    if (document.body && document.body.scrollTop && document.body.scrollLeft)
    {
       top  = document.body.scrollTop;
       left = document.body.scrollleft; 
    }
    if (document.documentElement && document.documentElement.scrollTop && document.documentElement.scrollLeft)
    {
       top  =  document.documentElement.scrollTop;
       left =  document.documentElement.scrollLeft; 
    }

## 元素在页面中的绝对定位

![enter description here][1]
![enter description here][2]

  [1]: ./images/Image%202.png "Image 2.png"
  [2]: ./images/Image%203.png "Image 3.png"
