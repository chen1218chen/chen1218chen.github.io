---
title: bootstrapTable总结
date: 2016-05-04 15:54:29
tags: bootstrap
---

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
# bootstrap model远程加载示例 #

    <a id="adduser" class="btn btn-danger" data-toggle="modal"	href="addUser.html" data-target="#adduserModal">添加</a>
    <div class="modal fade" id="adduserModal" role="dialog" aria-labelledby="myModalLabel" data-keyboard="true">
        <div class="modal-dialog" role="document">
        <div class="modal-content"></div>
        </div>
    </div>
  

> table表中修改某一条记录时，model弹出框获取记录值

    function editTerminal(row,index) {
    	index1=index;
    	$('#editTerminalModal').on('show.bs.modal',function(event) {
    		var button = $(event.relatedTarget);
    		var recipient = button.data('whatever');
    		var modal = $(this);
    		modal.find('.modal-title').text(recipient);
    		modal.find('#tid').val(row.tid);
    		modal.find('#type').val(row.type);
    		modal.find('#cid').val(row.cid);
    		modal.find('#uid').val(row.uid);
    		modal.find('#date').val(row.date);
    	})
    }


  [1]: ./images/Image%201.png "Image 1.png"