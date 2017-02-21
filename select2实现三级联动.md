---
title: select2实现三级联动
date: 2016-04-28 16:07:49
tags: [select2, Bootstrap]
---
最近根据项目需求要求实现数据分类，分类数据分为三级，考虑使用select2来实现三级联动效果
![enter description here][1]
## Model
	private Integer cid;
	private String cname;
	private String level;//所在层级
	private String parent;//夫类ID

## html
三个select标签，初始将后两个select隐藏   
**Tips小技巧:** 若使用$.show()，$.hide()方法无法直接将select隐藏，则将它的上层标签隐藏，或者包裹在span中，将span隐藏。
 
		<span id="one"> <select id="levelOne" name="levelOne" placeholder="分类" style="width: 100px">
		</select></span> 
		<span id="two" > <select id="levelTwo" name="levelTwo"
			placeholder="分类" style="width: 100px; display: none;">
		</select>
		</span> <span id="three"> <select id="levelThree" name="levelThree"
			placeholder="分类" style="width: 160px;  display: none;">
		</select>
		</span>
	
		<button type="button" class="btn btn-info" id="save"
			style="margin-top: 5px; padding: 3px 12px">
			<i class="">保存</i>
		</button>
	
## js
分解功能，首次加载有几级数据显示几级数据，点击一类数据，对应的展示二类数据及第一个二类数据对应的三类数据，没有的让其隐藏  

	$(function() {
		var oneselect,twoselect,threeselect;
		selectAll();
		selectInit();
	})
	//初始化加载
	function selectInit(){
		selectOne();
		
		$("#levelOne").on("select2:select", function (e) {
			var levelOne = $("#levelOne").val();
			selectTwo(levelOne);
		});

		$('#levelTwo').on("select2:select",function(){
			var levelTwo = $("#levelTwo").val();
			selectThree(levelTwo);
		});
	}
	//获取所有数据，分类保存到全局数组中，以方便后面使用
	function selectAll(){
		$.ajax({
			url : "rest/category/queryAll",
			type : "get",
			async: false,//注意，必须是同步，不然全局变量取不到值
			dataType : 'json',
			success : function(result) {
				var oneLevel = [];
				var twoLevel = [];
				var threeLevel = [];
				for (var i = 0; i < result.length; i++) {
					if (result[i].level == "1") {
						oneLevel.push(result[i]);
					} else if (result[i].level == "2") {
						twoLevel.push(result[i]);
					} else if (result[i].level == "3") {
						threeLevel.push(result[i]);
					}
				}
				oneselect = oneLevel;
				twoselect = twoLevel;
				threeselect = threeLevel;
			}
		});
	}
	
	//取第一类数据
	function selectOne(){

		select2Show("#levelOne", oneselect);
		
		selectTwo(oneselect[0].cid);
	}
	//取对应的第二层数据
	function selectTwo(parent){
		var twoTmp = [];
		for (var j = 0; j < twoselect.length; j++) {
			if (twoselect[j].parent == parent) {
				twoTmp.push(twoselect[j])
			}
		};
		if (twoTmp.length != 0) {
			$("#two").show();
			select2Show("#levelTwo", twoTmp);
			
			selectThree(twoTmp[0].cid);
			
		} else {
			twoHide();
			threeHide();
		}
	}
	//取对应的第三层数据
	function selectThree(parent){
		var threeTmp = [];
		for (var m = 0; m < threeselect.length; m++) {
			if (parent == threeselect[m].parent) {
				threeTmp.push(threeselect[m]);
			}
		};
		if (threeTmp.length != 0) {
			$("#three").show();
			select2Show("#levelThree", threeTmp);
		} else {
			threeHide();
		}
	}
	
	function twoHide(){
		$("#levelTwo").empty();
		$("#two").hide();
	}
	function threeHide(){
		$("#levelThree").empty();
		$("#three").hide();
	}
	function select2Show(id,result){
		if(result.length!=0){
			var data=[];
			
			for ( var i = 0; i < result.length; i++) {
				var tmp = {};
				tmp["id"] = result[i].cid;
				tmp["text"] = result[i].cname;
				data.push(tmp);
			}
			
			$(id).empty();
			$(id).select2({
				data : data,
			});
		}
	}

值得注意的是，某层类别需要隐藏时清空select2的数据，在保存分类时就可以获取类别id，如下代码所示
![enter description here][2]

	function save(){
		var ids = $.map($table.bootstrapTable('getSelections'), function(row) {
			return row.id;
		});
		
		if (ids == "") {
			alertView("请选择待归类信息");
			return;
		}
		alert(ids);
		if($("#levelThree").val()!=null){
			saveData($("#levelThree").val(),ids);
		}else {
			if($("#levelTwo").val()!=null){
				saveData($("#levelTwo").val(),ids);
			}else {
				saveData($("#levelOne").val(),ids);
			}
		}
	}

	function saveData(id,ids){
		//数据保存，将数据值用ajax传到后台，进行保存
		$.ajax({
			url:"rest/category/saveClassfi",
			type:"post",
			data:{
				"id":id,
				"ids":ids
			},
			dataType:"json",
			success:function(result){
				
			}
		})
	}
	
下图是获取到的待归类数据ID
![enter description here][3]
若未选择则弹出提示框：
![enter description here][4]


  [1]: ./images/1.png "1.png"
  [2]: ./images/Image%202.png "Image 2.png"
  [3]: ./images/Image%201.png "Image 1.png"
  [4]: ./images/Image%203.png "Image 3.png"