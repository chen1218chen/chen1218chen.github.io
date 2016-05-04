---
title: 使用BootstrapPaginator插件数据分页
date: 2016-04-14 14:38:49
tags: Bootstrap Paginator, Bootstrap
---
根据项目需要在信息展示列表页面进行分页显示由于数据时一次性取出分页展示，在数据量大的时候数据获取时速度慢的问题，后续改进取数据的方法，每次只取一页的数据。
# 居中显示
首先，在信息展示的列表页面上添加分页按钮

    <nav style="text-align: center">
        <ul class="pagination" id="pageUl"></ul>
    </nav>

# 前台控制分页
js中配置分页按钮参数，并进行启用。这种方法相当于把数据拿到前台，在前台控制分页

    var element = $('#pageUl');//对应下面ul的ID  
    var options = { 
    	size:"normal",
    	alignment:"center",
        bootstrapMajorVersion:3,  
        currentPage: 1,//当前页面  
        numberOfPages: 5,//一页显示几个按钮（在ul里面生成5个li）  
        totalPages:22, //总页数  
        itemTexts: function(type, page, current) { //修改显示文字
            switch (type) {
            case "first":
                return "第一页";
            case "prev":
                return "上一页";
            case "next":
                return "下一页";
            case "last":
                return "最后一页";
            case "page":
                return page;
            }
        }, 
        onPageClicked: function (event, originalEvent, type, page) { //换页
            paginator(msg,page);
        	$('#infolist').html('');
        	$('#infolist').append(str);
        },
    }  
    //分页启动
    element.bootstrapPaginator(options); 
    paginator(msg,1);
    //分页
    function paginator(arrData, page) {
    	var str="";
    	var name;
    	for (i = (page - 1) * 5; i < page * 5; i++) {
    		if(i<arrData.length){
    			var param = "content.jsp?id="+ arrData[i].id;
    			var content = arrData[i].content;
    			var s = content.substr(0, 7);
    			if(arrData[i].telephone==""){
    				name="匿名用户"
    			}
    			else{
    				name=arrData[i].telephone;
    			}
    		 str += '<div class="wz"><h3><a href="'
    				+ param
    				+ '" title="'
    				+ arrData[i].title
    				+ ""
    				+ '">'
    				+ s
    				+ "..."
    				+ '</a></h3><dl><dt>'
    				+ '<a href="'
    				+ param
    				+ '"  style="margin-top: 0px;" title="阅读全文"><img src="'
    				+ 'picture/'+arrData[i].picturepath1
    				+ '" width="200" height="123" alt=""></a></dt><dd>'
    				+ '<p class="dd_text_1">'
    				+ arrData[i].content
    				+ '</p><p class="dd_text_2">'
    				+ '	<span class="left author" >'
    				+ name
    				+ '</span><span class="left sj" style="padding-left:80px;">'
    				+ arrData[i].dataTime
    				+ '</span>'
    				+ '<span class="left yd"><a href="'
    				+ param
    				+ '"  title="阅读全文" style="margin-left: auto";>阅读全文</a></span>'
    				+ '<div class="clear"></div></p></dd><div class="clear"></div></dl></div>';
    			
    			$('#infolist').html(''); 
    			$('#infolist').append(str);
    		}
    		else {
    			break;
    		}
    	}
    }

# 后台分页控制
1. MySQL分页语句
		
		"select * from test.uploadinfo order by id desc limit "+ ((pageNow-1)*pageSize)+","+pageSize;

2. rest服务      
	
		@RequestMapping(value = "/onePageData", method = RequestMethod.POST)
		@ResponseBody
		public JSONArray onePageData(@RequestParam("pageNow") String pageNow, @RequestParam("pageSize") int pageSize) {
			List<Uploadinfo> itemList = itemServiceImpl.onePage(Integer.parseInt(pageNow), pageSize);
			JSONArray itemJSONList = JSONArray.fromObject(itemList, Common.getJsonConfig());
			return itemJSONList;
		}
	
3. service 

sql语句

	@Override
	public List<Uploadinfo> onePage(int pageNow, int pageSize) {
		// TODO Auto-generated method stub
		String sql = "select * from test.uploadinfo order by id desc limit "+ ((pageNow-1)*pageSize)+","+pageSize*pageNow;
		List<Uploadinfo> infolist = itemDaoImpl.getSQLQuery(sql).list();
		return infolist;
	}

hibernate中调用原生SQL语句，ajax数据回调报500错误，转成JSONArray返回数据时，不是对象类型前台无法识别数据，导致不知道rest服务是否调用成功，从而报500。转而使用ＨＱＬ语句查询即可解决。  
HQL语句中limit无效还是会把所有数据查出来，不支持limit，替代方法如下 

	@Override
	public List<Uploadinfo> onePage(int pageNow, int pageSize) {
		// TODO Auto-generated method stub
		String sql = "from Uploadinfo";
		Query query = itemDaoImpl.getQuery(sql);
		query.setFirstResult((pageNow-1)*pageSize);
		query.setMaxResults(pageSize*pageNow);
		List<Uploadinfo> infolist = query.list(); 
		return infolist;
	}
 
js调用
		
	$(function(){
		queryAll("rest/Uploadinfo/totlePages");
	})
	function queryAll(url){
	$.ajax({
		type : "GET",
		url :  url,
		dataType : "text",
		success : function(msg) {
			var pages = Math.ceil(msg/5); 
			var element = $('#pageUl');//对应下面ul的ID  
		    var options = { 
		    	size:"normal",
		    	alignment:"center",
		        bootstrapMajorVersion:3,  
		        currentPage: 1,//当前页面  
		        numberOfPages: 5,//一页显示几个按钮（在ul里面生成5个li）  
		        totalPages: pages, //总页数  
		        itemTexts: function(type, page, current) { //修改显示文字
		            switch (type) {
		            case "first":
		                return "第一页";
		            case "prev":
		                return "上一页";
		            case "next":
		                return "下一页";
		            case "last":
		                return "最后一页";
		            case "page":
		                return page;
		            }
		        }, 
		        onPageClicked: function (event, originalEvent, type, page) { //换页
		        	paginator(page);
		        },
		    }  
		    //分页启动
		   element.bootstrapPaginator(options); 
		   paginator(1);
		}
	});
	}

	//分页
	function paginator(pageNow) {
	$.ajax({
		type : "post",
		url :  "rest/Uploadinfo/onePageData",
		dataType : "json",
		data:{
			"pageNow":pageNow,
			"pageSize":5
		},
		success : function(arrData) {
			console.dir(arrData)
			var str="";
			var name;
			for(var i=0; i<5; i++){
				if(i<arrData.length){
					var param = "content.jsp?id="+ arrData[i].id;
					var content = arrData[i].content;
					var s = content.substr(0, 7);
					if(arrData[i].telephone==""){
						name="匿名用户"
					}
					else{
						name=arrData[i].telephone;
					}
					str += '<div class="wz"><h3><a href="'
						+ param
						+ '" title="'
						+ arrData[i].title
						+ ""
						+ '">'
						+ s
						+ "..."
						+ '</a></h3><dl><dt>'
						+ '<a href="'
						+ param
						+ '"  style="margin-top: 0px;" title="阅读全文"><img src="'
						+ 'picture/'+arrData[i].picturepath1
						+ '" width="200" height="123" alt=""></a></dt><dd>'
						+ '<p class="dd_text_1">'
						+ arrData[i].content
						+ '</p><p class="dd_text_2">'
						+ '	<span class="left author" >'
						+ name
						+ '</span><span class="left sj" style="padding-left:80px;">'
						+ arrData[i].dataTime
						+ '</span>'
						+ '<span class="left yd"><a href="'
						+ param
						+ '"  title="阅读全文" style="margin-left: auto";>阅读全文</a></span>'
						+ '<div class="clear"></div></p></dd><div class="clear"></div></dl></div>';
					
					$('#infolist').html(''); 
					$('#infolist').append(str);
				}
			}
		}
	});
}