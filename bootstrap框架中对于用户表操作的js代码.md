---
title: bootstrap框架中对于用户表操作的js代码
date: 2016-04-28 16:45:41
tags: bootstrap
---
采用requireJS实现js的模块化开发，集成各种bootstrap插件来实现对用户数据的管理
![enter description here][1]
添加功能：
![enter description here][2]
修改功能（类似下图）：
![enter description here][3]

main.js

    require.config({
    //	baseUrl : 'js',
    	paths : {
    		jquery : 'lib/jquery.min',
    		'css' : 'lib/css.min',
    		'common' : 'app/common',
    		'table' : 'app/table',
    		'role' : 'app/role',
    		'bootstrap' : 'lib/bootstrap.min',
    		'bootstrapTable' : 'lib/bootstrap-table',
    		'tableExport' : 'lib/tableExport',
    		'tableLanguage' : 'lib/bootstrap-table-zh-CN',
    		'bootstrapTableExport' : 'lib/bootstrap-table-export',
    		'bootstrap-dialog' : 'lib/bootstrap-dialog.min',
    		'jquery.base64' : 'lib/jquery.base64',
    		'confirm' : 'lib/jquery-confirm.min',
    		'bootstrapValidator' : 'lib/bootstrapValidator.min',
    		'select2' : 'lib/select2.full.min'
    	},
    	shim : {
    		'jquery' : {
    			init : function() {
    				return jquery.noConflict(true);
    			},
    			exports : 'jquery'
    		},
    		'common' : {
    			deps : [ 'jquery', 'bootstrap',
    					'bootstrapValidator', 'select2', 'confirm' ,'bootstrap-dialog'],
    			exports : 'common'
    		},
    		'table' : {
    			exports : 'table'
    		},
    		'bootstrap' : {
    			deps : [ 'jquery','css!../css/bootstrap.min.css' ],
    			exports : "$.fn.popover"
    		},
    		'bootstrapTable' : {
    			deps : [ 'jquery', 'bootstrap', 'css!../css/bootstrap-table.css'],
    			exports : '$.fn.bootstrapTable'
    		},
    		'tableExport' : {
    			deps : [ 'jquery' ],
    			exports : '$.fn.extend'
    		},
    		'tableLanguage' : {
    			deps : [ 'jquery', 'bootstrapTable' ],
    			exports : '$.fn.bootstrapTable.defaults'
    		},
    		'bootstrapTableExport' : {
    			deps : [ 'jquery', 'bootstrapTable' ],
    			exports : '$.fn.bootstrapTable.defaults'
    		},
    		'bootstrap-dialog' : {
    			deps:['css!../css/bootstrap-dialog.min.css'],
    		},
    		'confirm' : {
    			deps : [ 'jquery' ,'css!../css/jquery-confirm.min.css'],
    			exports : 'confirm'
    		},
    		'select2' : {
    			deps : [ 'jquery','css!../css/select2.min.css' ],
    			exports : 'select2'
    		},
    		'bootstrapValidator' : {
    			deps : [ 'jquery', 'css!../css/bootstrapValidator.min.css'],
    			exports : '$.fn.bootstrapValidator'
    		}
    	},
    	waitSeconds : 0,
    //	enforceDefine : true,
    // skipDataMain:true
    });
    function operateFormatter(value, row, index) {
    	return [
    			'<a class="edit ml10" id="edit" href="#" data-toggle="modal" data-target="#editUserModal" title="Edit" data-whatever="修改终端">',
    			'<i class="glyphicon glyphicon-edit"></i>', '</a>&nbsp;' ].join('');
    }
    require([ 'jquery', 'bootstrap-dialog', 'common', 'table', 'lib/domReady!'],
    		function(jquery, BootstrapDialog, common, userTable, domReady) {
    			userTable.init();
    		/*	if ($('.glyphicon-user').text() != "admin") {
    				$('#custom-toolbar').children().attr("disabled", "true").css({
    					"background-color" : "#DEDEDE",
    					"border-color" : "#DCDCDC"
    				});
    				$("#edit").attr("disabled","true").css({
    					"background-color" : "#DEDEDE",
    					"border-color" : "#DCDCDC"
    				});
    			}*/
    			
    			window.operateEvents = {
    				'click .edit' : function(e, value, row, index) {
    					userTable.editGroup(row, index);
    				},
    				'click .remove' : function(e, value, row, index) {
    					userTable.deleteOneGroup(row.uid);
    				}
    			};
    			$('#adduser').click(function() {
    				common.openDialog('添加用户', 'addUser.html', false);
    			});
    			$('#deluser').click(function() {
    				userTable.deleteUser();
    			});
    			$("#logout").click(function() {
    				common.logout();
    			});
    			$('#changePasswd').click(function() {
    				common.changePasswd();
    			});
    		});
    		
table.js 主要功能实现

    define([ 'jquery', 'bootstrap','tableExport', 'bootstrapTable', 'bootstrapValidator',
    		'select2', 'bootstrap-dialog', 'confirm', 'common', 'tableLanguage'
    		, 'bootstrapTableExport','role' ], function(jquery, bootstrap,tableExport,
    		bootstrapTable, bootstrapValidator, select2, BootstrapDialog, confirm,
    		common,tableLanguage,bootstrapTableExport,role) {
    	var userTable = {};
    	common.initPage();
    	userTable.init = function() {
    		userTable.queryAll();
    		$table = $('#table');
    		common.organList("#oid-change");
    		userTable.selectUser();
    		common.role("#uduty");
    	};
    	
    	userTable.selectUser = function(){
    		$.getJSON("js/config.json", function(result) {
    			var approval = result.approval;
    			$("#isapproval").select2({
    				data : approval,
    				minimumResultsForSearch : -1,// 禁用搜索
    				placeholder : "审批",
    				allowClear : true
    			});
    		});
    	};
    	
    	userTable.addUser = function(form) {
    		$.ajax({
    			url : "/JDJ/rest/user/saveUser",
    			type : "post",
    			dataType : 'json',
    			data : $(form).serializeArray(),
    			success : function(result) {
    				$('#adduserModal').modal('hide');
    				if (result) {
    					common.alertConfirm("添加成功!");
    				} else {
    					common.alertView("添加失败！");
    				}
    			},
    		});
    	};
    	userTable.deleteUser = function() {
    		var ids = $.map($table.bootstrapTable('getSelections'), function(row) {
    			return row.uid;
    		});
    		
    		if (ids == "") {
    			common.alertView("请选择删除项");
    			return;
    		}
    		userTable.delUser(ids);
    	};
    	userTable.deleteOneUser = function(ids) {
    		$table.bootstrapTable('remove', {
    			field : "id",
    			values : ids
    		});
    		userTable.delUser(ids);
    	};
    	userTable.delUser = function(ids) {
    		$.ajax({
    			url : "../rest/user/delUser",
    			type : "post",
    			traditional : true,// 这样就能正常发送数组参数了
    			data : {
    				"ids" : ids,
    			},
    			success : function(result) {
    				if (result) {
    					common.alertView("删除成功！！");
    					$table.bootstrapTable('remove', {
    						field : "uid",
    						values : ids
    					});
    				} else
    					common.alertView("删除失败！！");
    			},
    			error : function() {
    				common.alertView("删除失败！！");
    			}
    		});
    	};
    	userTable.queryAll = function() {
    		$.ajax({
    			url : "../rest/user/queryAllUser",
    			type : "get",
    			dataType : 'json',
    			success : function(data) {
    				if (data != null) {
    					userTable.OwnerName(data);
    				}
    			},
    			error:function(){
    				common.alertView("数据返回出错");
    			}
    		});
    	};
    	userTable.OwnerName = function(list) {
    		var arraylist = [];
    		for ( var i = 0; i < list.length; i++) {
    			var obj = {};
    			obj["uid"] = list[i].uid;
    			obj["uname"] = list[i].uname;
    			obj["uphone"] = list[i].uphone;
    			obj["upassword"] = list[i].upassword;
    			obj["oid"] = list[i].organ.oid;
    			obj["oname"] = list[i].organ.oname;
    			obj["rid"] = list[i].roles[0].rid;
    			obj["rname"] = list[i].roles[0].rname;
    			if(list[i].isapproval=="1"){
    				obj["isapproval"] = "通过";
    			}
    			else{
    				obj["isapproval"] = "未通过";
    			}
    			arraylist.push(obj);
    		}
    		$('#table').bootstrapTable({
    			data : arraylist
    		});
    	};
    	
    	//open edit modal
    	userTable.editGroup = function(row, index) {
    		index1 = index;
    		$('#editUserModal').on('show.bs.modal', function() {
    			console.dir(row);
    			var modal = $(this);
    			modal.find('#uid').val(row.uid);
    			modal.find('#uname').val(row.uname);
    			modal.find('#uname').val(row.uname);
    			modal.find('#uduty').val(row.rid);
    			$("#uduty").val(row.rid).trigger("change");
    			modal.find('#oid-change').val(row.oid);
    			$("#oid-change").val(row.oid).trigger("change");
    			modal.find('#upassword').val(row.upassword);
    			modal.find('#uphone').val(row.uphone);
    			if((row.isapproval)=="通过"){
    				modal.find('#isapproval').val("1");
    				$("#isapproval").val("1").trigger("change");
    			}else{
    				modal.find('#isapproval').val("0");
    				$("#isapproval").val("0").trigger("change");
    				
    			}
    			
    			$("#editUserSubmit").click(function(){
    				userTable.edit();
    			});
    		});
    	};
    	
    	//edit submit
    	userTable.edit = function() {
    //		var uid = $('#uid').val();
    		var uname = $('#uname').val();
    		if(uname==""){
    			common.alertView("用户名不能为空");
    			return;
    		}
    //		var upassword = $('#upassword').val();
    		var oid = $('#oid-change').val();
    		var uphone = $('#uphone').val();
    		var uduty = $('#uduty').val();
    		var isapproval = $('#isapproval').val();
    		$table.bootstrapTable('updateRow', {
    			index : index1,
    			row : {
    				uname : uname,
    //				upassword : upassword,
    				oid : oid,
    				uphone : uphone,
    				uduty : uduty,
    				isapproval: isapproval
    			}
    		});
    		
    		$.ajax({
    			type : "post",
    			url : "../rest/user/updateUser",
    			cache : false,
    			dataType : 'json',
    			data : $("#editUserForm").serializeArray(),
    			success : function(result) {
    				$('#editUserModal').modal('hide');
    				if (result) {
    					common.alertConfirm("修改成功！！");
    				} else {
    					common.alertView("修改失败！");
    				}
    			},
    			error : function() {
    				$('#editUserModal').modal('hide');
    				common.alertView("修改失败！");
    			}
    		});
    	};
    	
    	//edit valiator
    	userTable.valiatorEditUser = function() {
    		$('#editUserForm').bootstrapValidator({
    			message : '无效值',
    			feedbackIcons : {
    				valid : 'glyphicon glyphicon-ok',
    				invalid : 'glyphicon glyphicon-remove',
    				validating : 'glyphicon glyphicon-refresh'
    			},
    			submitHandler : function(validator, form, submitButton) {
    				userTable.edit(form);
    			},
    			fields : {
    				uname : {
    					validators : {
    						notEmpty : {
    							message : '用户名不能为空'
    						}
    					}
    				},
    				upassword : {
    					validators : {
    						notEmpty : {
    							message : '密码不能为空'
    						},
    						stringLength : {
    							min : 1,
    							max : 30,
    							message : '密码长度为1到30位'
    						}
    					}
    				},
    				uphone : {
    					validators : {
    						notEmpty : {
    							message : '电话不能为空'
    						},
    						regexp : {
    							regexp : /^[0-9]+$/,
    							message : '格式不正确'
    						}
    					}
    				},
    			}
    		});
    	};
    	
    	//add user valiator
    	userTable.valiatorAddUser = function() {
    		$('#addForm').bootstrapValidator({
    			message : '无效值',
    			feedbackIcons : {
    				valid : 'glyphicon glyphicon-ok',
    				invalid : 'glyphicon glyphicon-remove',
    				validating : 'glyphicon glyphicon-refresh'
    			},
    			submitHandler : function(validator, form, submitButton) {
    				userTable.addUser(form);
    			},
    			fields : {
    				uname : {
    					validators : {
    						notEmpty : {
    							message : '用户名不能为空'
    						}
    					/*
    					 * , regexp : { regexp : /^[a-zA-Z0-9_\.]+$/, message :
    					 * '用户名只能由字母，数字，圆点和下划线组成' }
    					 */
    					}
    				},
    				upassword : {
    					validators : {
    						notEmpty : {
    							message : '密码不能为空'
    						},
    						stringLength : {
    							min : 1,
    							max : 30,
    							message : '密码长度为1到30位'
    						},
    						identical : {
    							field : 'password2',
    							message : '密码与确认密码不一致'
    						},
    					}
    				},
    				password2 : {
    					validators : {
    						notEmpty : {
    							message : '确认密码不能为空'
    						},
    						stringLength : {
    							min : 1,
    							max : 30,
    							message : '密码长度为1到30位'
    						},
    						identical : {
    							field : 'upassword',
    							message : '密码与确认密码不一致'
    						}
    					}
    				},
    				uphone : {
    					validators : {
    						notEmpty : {
    							message : '电话不能为空'
    						},
    						regexp : {
    							regexp : /^[0-9]+$/,
    							message : '格式不正确'
    						}
    					}
    				},
    			}
    		});
    	};
    	return userTable;
    });


  [1]: ./images/%E5%9B%BE%E7%89%871.png "图片1.png"
  [2]: ./images/%E5%9B%BE%E7%89%872.png "图片2.png"
  [3]: ./images/%E5%9B%BE%E7%89%873.png "图片3.png"