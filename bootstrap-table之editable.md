---
title: bootstrap-table之editable
date: 2017-01-11 10:24:33
tags: bootstrap table
---
## editable
bootstrap table的修改，只需要引入
- bootstrap-table-editable.js
- bootstrap-editable.js
- bootstrap-editable.css即可。

列属性上添加`data-editable="true"`


    <th data-field="address" data-align="center" data-valign="middle" data-editable="true">地址</th>
    
bootstrap table 插件扩展了X-editable功能。
修改table属性触发**editable-save.bs.table**事件。

    $('#table').on('editable-save.bs.table', function (field, row, oldValue, $el) {
    	editSave(row,oldValue,name);
    });
