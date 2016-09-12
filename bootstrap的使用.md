---
title: bootstrap的使用
date: 2016-09-09 17:13:17
tags: Bootstrap
---
[toc]

## bootstrap model远程加载

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
