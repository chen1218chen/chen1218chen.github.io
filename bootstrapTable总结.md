---
title: bootstrap table小技巧
date: 2016-05-04 15:54:29
tags: bootstrap
---
[TOC]
## table参数（Table options）
The table options are defined in **jQuery.fn.bootstrapTable.defaults**.

按照某列排序:

    data-sort-name="dataTime" data-sort-order="desc"
   
查看细节,效果如下图所示

    data-detail-view="true" data-detail-formatter="detailFommater"

    function detailFommater(index, row, element) {
    	return 'ccc'+row;
    }

![enter description here][1]
## 手动启动
如果bootstrapTable没有启用，选择手动触发。

    $('#table').bootstrapTable();
## pageNum初始化

    data-page-list="[10, 25, 50, 100, 'ALL']"

## 高度自适应

    $(function(){
    	$table = $('#table');
    	$table.bootstrapTable('resetView', {
    	    height: getHeight()
        });
    })
    function getHeight() {
        return $(window).height() - $('h1').outerHeight(true);
    }
或者

    $(window).resize(function() {
        $('#table').bootstrapTable('resetView');
    });
table的高度可以自己设定
    
    data-height="300px"
## 手动加载table值

    $('#table').bootstrapTable({
    	data : result
    });
## table data from url

> html代码


    <table data-toggle="table" data-url="rest/Uploadinfo/queryAll" data-show-refresh="true">
> 或者在JS中用URL地址获取数据，与data-url的作用相同：
> 方法一


    $('#table').bootstrapTable({url:"rest/Uploadinfo/queryAll"});
> 方法二


    $.get("rest/Uploadinfo/queryAll",function(result){
    	$('#table').bootstrapTable({
    		data : result
    	});
    })
> 后台java代码：


    @RequestMapping(value = "/queryAll", method = RequestMethod.GET)
    @ResponseBody
        public JSONArray queryAll() {
        	List<Uploadinfo> itemList = itemServiceImpl.queryAll();
        	JSONArray jsonArr = new JSONArray();
        	for(int i=0;i<itemList.size();i++){
    		JSONObject obj  = new JSONObject();
    		obj.put("id", itemList.get(i).getId());
    		obj.put("content", itemList.get(i).getContent());
    		obj.put("telephone", itemList.get(i).getTelephone());
    		obj.put("picturepath1", itemList.get(i).getPicturepath1());
    		String date = Common.dateFormatStr(itemList.get(i).getDataTime()); 
    		
    		obj.put("dataTime", date);
    		obj.put("cname", itemList.get(i).getClassfic().getCname());
    		jsonArr.add(obj);
    	}
    	return jsonArr;
    }
这样页面一打开就可以加载数据，且reload按钮可以发挥作用，不用在js页面中写queryAll()，试用ajax来异步获取数据。直接在后台"rest/Uploadinfo/queryAll"服务中封装好要用的数据
![数据加载][2]

## 数据重新加载


    $('#table').bootstrapTable('load', data);
  
## 更新操作
```
	$table.bootstrapTable('updateRow', {
		index : index1,
		row : {
			tid : tid,
			cid : cid,
			uid : uid,
			date : date,
			type : type,
		}
	});
```
## 添加操作

>使用bootstrap table插件
>table rowStyle和runningFormatter


    <table data-row-style="rowStyle"> <thead> <tr><th data-align="center" data-formatter="runningFormatter">ID</th> </tr></thead>

    function rowStyle(row, index) {
    	var classes = ['success', 'info', 'warning', 'danger'];
     /*if (index % 2 === 0 && index / 2 < classes.length) {
        return {
            classes: classes[index / 2]
        };
    }*/
    if(index % 2 ===0){
    	return {
    		classes:classes[index%3]
    	}
    }
    return {};
    }
    function runningFormatter(value, row, index) {
        return index+1;
    }


> tablet添加操作


    <th data-field="operate" data-formatter="operateFormatter"
    data-events="operateEvents" data-align="center">操作</th>

## editable
bootstrap table的修改，只需要引入bootstrap-table-editable.js、bootstrap-editable.js、bootstrap-editable.css即可。bootstrap table 插件扩展了X-editable功能。
修改table属性触发**editable-save.bs.table**事件。

    $('#table').on('editable-save.bs.table', function (field, row, oldValue, $el) {
    	data.changeField(row,oldValue,name);
    });


  [1]: ./images/Image%201.png "Image 1.png"
  [2]: ./images/2.png "2.png"