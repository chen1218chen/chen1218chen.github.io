---
title: js中毫秒转化成想要的时间格式
date: 2017-08-08 11:11:42
tags: JavaScript
---

从后台传一个Date()数据类型

![前台接受到的Date()类型数据][1]

到前台进行格式化处理

    var newTime = new Date(dateTime.time);

    > Mon Aug 07 2017 15:01:28 GMT+0800 (中国标准时间)
在对`newTime`进行格式化
 

    Date.prototype.format = function(fmt) { 
     var o = { 
        "M+" : this.getMonth()+1,                 //月份 
        "d+" : this.getDate(),                    //日 
        "h+" : this.getHours(),                   //小时 
        "m+" : this.getMinutes(),                 //分 
        "s+" : this.getSeconds(),                 //秒 
        "q+" : Math.floor((this.getMonth()+3)/3), //季度 
        "S"  : this.getMilliseconds()             //毫秒 
    }; 
    if(/(y+)/.test(fmt)) {
            fmt=fmt.replace(RegExp.$1, (this.getFullYear()+"").substr(4 - RegExp.$1.length)); 
    }
     for(var k in o) {
        if(new RegExp("("+ k +")").test(fmt)){
             fmt = fmt.replace(RegExp.$1, (RegExp.$1.length==1) ? (o[k]) : (("00"+ o[k]).substr((""+ o[k]).length)));
         }
     }
    return fmt; 
    }

    var time1 = newTime.format("yyyy-MM-dd hh:mm:ss");
得到

    > 2017-08-07 15:01:28

  [1]: ./images/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20170808111517.png "微信截图_20170808111517.png"