---
title: hibernate之翻页的实现
date: 2016-06-13 10:46:36
tags: [Hibernate, java]
---
pageSize:每页记录数
pageNow：当前页码

    @Override
    public List<Uploadinfo> onePage(int pageNow, int pageSize,String type) {
    	// TODO Auto-generated method stub
    	String sql;
    	if("picture".equals(type)){
    		 sql = "from Uploadinfo where picturepath1 <> '' and picturepath1 <> 'picture/meizhaopian.png' order by dataTime desc";
    		 
    	}else{
    		sql="from Uploadinfo order by dataTime desc";
    	}
    	Query query = itemDaoImpl.getQuery(sql);
    	//设置起始页
    	query.setFirstResult((pageNow - 1) * pageSize);
    	//设置每页最大记录数
    	query.setMaxResults(pageSize);
    	List<Uploadinfo> infolist = query.list();
    	return infolist;
    }
    
index.jsp
bootstrap中的分页组件

    <nav style="text-align: center">
    	<ul class="pagination" id="pageUl"></ul>
    </nav>

index.js

    $(window).load(function() {
        ////获取记录条数
    	queryAll("rest/Uploadinfo/totlePages");
    });
    function queryAll(url){
    	$.ajax({
    		type : "post",
    		url :  url,
    		dataType : "text",
    		data:{
    		     "type":""
    		},
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
    		        onPageClicked: function (event, originalEvent, type, page) { 
    		            //换页,下一页
    		        	paginator(page);
    		        },
    		    }  
    		    //分页启动
    		   element.bootstrapPaginator(options); 
    		   paginator(1);
    		}
    	});
    }
    function paginator(pageNow) {
    	$.ajax({
    		type : "post",
    		url :  "rest/Uploadinfo/onePageData",
    		dataType : "json",
    		data:{
    			"pageNow":pageNow,
    			"pageSize":5,
    			"type":""
    		},
    		success : function(arrData) {
    			console.dir(arrData)
    			var str="";
    			var name;
    //			for (i = (page - 1) * 5; i < page * 5; i++) {
    			for(var i=0; i<arrData.length; i++){
    				    var pictureUrl = arrData[i].picturepath1;
    					var param = "content.jsp?id="+ arrData[i].id;
    					var content = arrData[i].content;
    					var s = content.substr(0, 7);		
    					if(arrData[i].telephone==""){
    						name="匿名用户"
    					}
    					else{
    						name=arrData[i].telephone;
    					}
    					str += '<div class="wz"><h3 style="width:95%;"><a href="'
    						+ param
    						+ '" title="'
    						+ arrData[i].telephone
    						+ ""
    						+ '">'
    						+ s
    						+ "..."
    						+ '</a></h3><dl><dt>'
    						+ '<a href="'
    						+ param
    						+ '"  style="margin-top: 0px;" title="阅读全文"><img src="'
    						+ pictureUrl
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
    	});
    }
onePageData中调用onePage方法来实现分页。

java 

    //获取记录条数
    @RequestMapping(value = "/totlePages", method = RequestMethod.POST)
    @ResponseBody
    public int totlePages(@RequestParam("type") String type) {
    	List<Uploadinfo> itemList = itemServiceImpl.totalPage(type);
    	int size = itemList.size();
    	return size;
    }