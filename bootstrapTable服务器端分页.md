---
title: bootstrapTable服务器端分页
date: 2017-10-17 09:45:50
tags: [bootstrap table]
---

前几天项目中使用的bootstrapTable出错，检查后才发现随着项目运行数据越来越多，一次性将所有数据返回到前台进行分页出现问题，rest请求数据有限制，超过这个限制值的数据将会抛弃，导致返回到前台的数据不完整，系统出错。
为解决这个问题尝试研究了下bootstrapTable插件的server端分页功能，更深入的了解这个插件的使用。
## table的动态生成

    <table id="agencyTable" class="table table-hover"></table>

    $('#agencyTable').bootstrapTable({
    	method: 'post',
    	contentType: "application/x-www-form-urlencoded",//必须要有！！！！
    	url: "../rest/Uploadinfo/queryAgencyPage",//要请求数据的文件路径
    	height: 500,
    	toolbar: '#toolbar',//指定工具栏
    	striped: true, //是否显示行间隔色
    	dataField: "rows",
    	pageNumber: 1, //初始化加载第一页，默认第一页
    	pagination:true,//是否分页
    	queryParamsType:'limit',//查询参数组织方式
    	queryParams:queryParams,//请求服务器时所传的参数
    	sidePagination:'server',//指定服务器端分页
    	pageSize:5,//单页记录数
    	pageList:[5,10,20,30],//分页步进值
    	showRefresh:true,//刷新按钮
    	showColumns:true,
    	clickToSelect: true,//是否启用点击选中行
    	toolbarAlign:'right',//工具栏对齐方式
    	buttonsAlign:'right',//按钮对齐方式
    	toolbar:'#toolbar',//指定工作栏
    	columns:[
    	         {
    	        	 title:'全选',
    	        	 field:'state',
    	        	 checkbox:true,
    	        	 width:25,
    	        	 align:'center',
    	        	 valign:'middle'
    	         },
    	         {
    	        	 title:'id',
    	        	 field:'id',
    	        	 align:'center',
    	        	 visible:false
    	         },
    	         {
    	        	 title:'投诉信息',
    	        	 field:'dataTime',
    	        	 sortable:true,
    	        	 formatter:	function(value,row,index){
    	        			var param = "content.jsp?id=" + row.id;
    	        			var title = "";
    	        			var address = row.address;
    	        			if(address==""){
    	        				address="未知位置"
    	        			}
    	        			var telehone = row.telephone;
    	        			if (telehone == "")
    	        				telehone = "匿名用户";
    	        			
    	        			var user = row.name;
    	        			return '<div style="line-height: 26px;"><i class="glyphicon glyphicon-user" style="color:#5bc0de;"></i> '
    	        					+ user
    	        					+'<br /><i class="glyphicon glyphicon-earphone" style="color:#5bc0de;"></i> '
    	        					+ telehone
    	        					+'<br /><i class="glyphicon glyphicon-home" style="color:#5bc0de;"></i> '
    	        					+ address 
    	        					+ '</div>';
    	        		}
    	         },
    	         {
    	        	 title:'投诉图片',
    	        	 field:'picturepath1',
    	        	 sortable:true,
    	        	 formatter:function(value, row, index) {
    	        			var param = "content.jsp?id=" + row.id;
    	        			if (value != "")
    	        				return '<div><a  ><img src='
    	        						+ value
    	        						+ ' width="200px" height="133px"  style="padding:0px;" alt=""></a></div>';
    	        			else
    	        				return "无图片";
    	        		}
    	         },
    	         {
    	        	 title:'投诉内容',
    	        	 field:'content',
    	        	 align:'left',
    	        	 valign:'top',
    	        	 formatter:function(value, row, index) {
    	        			if (value == "") {
    	        				value = "举报人未说明具体情况"
    	        			} else {
    	        				if (value.length > 60) {
    	        					value = value.substring(0, 60) + "...";
    	        				}
    	        			}
    	        			var dataTime = row.dataTime;
    	        			return '<div style="height:60px;"><a href="#"'
    	        					+' style="line-height: 1.4em;"><h4 class="media-heading">'
    	        					+ value
    	        					+ '</a></div>'
    	        					+ '<div style="line-height: 26px;"><i class="glyphicon glyphicon-time" style="color:#5bc0de;"></i> '
    	        					+ dataTime+'</div>';
    	        		}
    	         },
    	         {
    	        	 title:'处理人',
    	        	 field:'process',
    	        	 align:'center',
    	        	 valign:'middle',
    	        	 formatter:function(value,row,index){
    	        		 return row.dealName.userName
    	        	 }
    	         }
             ],
             locale:'zh-CN',//中文支持,
             responseHandler:function(res){
            	 //在ajax获取到数据，渲染表格之前，修改数据源
            	 console.dir(res);
            	 return {
        	        total : result.total, //总页数,前面的key必须为"total"
        	        rows : result.rows //行数据，前面的key要与之前设置的dataField的值一致.
        	    };
             }
    })
    //请求服务数据时所传参数
    function queryParams(params){
    	console.log(params);
    	return{
    		//每页多少条数据
    		pageSize: params.limit,
    		//请求第几页
    		pageNumber:params.offset/params.limit+1, //当前页面,默认是上面设置的1(pageNumber)
    		searchText:$('#search_name').val()
    	}
    }
    //查询按钮事件
    $('#search_btn').click(function(){
    	$('#agencyTable').bootstrapTable('refresh', {url: '../rest/Uploadinfo/queryAgencyCount'});
    })


![请求到的数据][1]
- `responseHandler`：在ajax获取到数据，渲染表格之前，修改数据源。如果跟后端约定好，返回的数据格式第一层包含“rows”(行数据)和“total”(总数)。responseHandler可以不用写。
- `dataField`:bootstrap table 可以前端分页也可以后端分页，这里我们使用的是后端分页，后端分页时需返回含有total：总记录数,这个键值好像是固定的。rows： 记录集合 键值可以修改  dataField 自己定义成自己想要的就好
- `formatter`：对定义的每一列进行格式化。


    {
        ...
        formatter:'infoFormatter'
    }

    function infoFormatter(value,row,index){
        ...
    }
注意后面直接写格式化函数，如上面这种写法，写成函数名，引用别处定义的函数不起作用。


## 后端分页

    @RequestMapping(value = "/queryAgencyPage", method = RequestMethod.POST)
    @ResponseBody
    public JSONObject pageOne(Integer pageSize, Integer pageNumber, String searchText, HttpServletRequest request,
            HttpServletResponse response) throws JsonMappingException, IOException {
        //搜索框功能
        //当查询条件中包含中文时，get请求默认会使用ISO-8859-1编码请求参数，在服务端需要对其解码
        if (null != searchText) {
            try {
                searchText = new String(searchText.getBytes("ISO-8859-1"), "UTF-8");
            } catch (Exception e) {
                e.printStackTrace();
            }
        }

        //根据查询条件，获取符合查询条件的数据总量
        int total;
        List<Uploadinfo> list;
        ObjectMapper mapper = new ObjectMapper();
        mapper.setSerializationInclusion(Include.NON_NULL);
        JSONArray itemJSONList = new JSONArray();
        if(searchText != null){        	
        	total = itemServiceImpl.searchInfos(searchText).size();
        }else {
        	List<Uploadinfo> totleList = itemServiceImpl.queryAll();
    		total = totleList.size();
        }
        list = itemServiceImpl.queryByPage(pageSize, pageNumber, searchText);
        itemJSONList = JSONArray.fromObject(list, Common.getJsonConfig());
        //自己封装的数据返回类型，bootstrap-table要求服务器返回的json数据必须包含：totle，rows两个节点        
        JSONObject obj = new JSONObject();
        obj.accumulate("total", total);
        obj.accumulate("rows", mapper.writeValueAsString(itemJSONList));
        return obj;
    }

    public List<Uploadinfo> queryByPage(int pageSize, int pageNow ,String searchText) {
    	// TODO Auto-generated method stub
    	String hql;
    	Query query;
    	if(searchText == null || searchText == ""){
    		hql = "from Uploadinfo order by dataTime desc";
    		query = itemDaoImpl.getQuery(hql);
    	}else {			
    		hql = "from Uploadinfo where content like :value or telephone like :value or datetime like :value order by dataTime desc";
    		query = itemDaoImpl.getQuery(hql);
    		query.setParameter("value", '%'+searchText+'%');
    	}
    	query.setFirstResult((pageNow - 1) * pageSize);
    	query.setMaxResults(pageSize);
    	List<Uploadinfo> infolist = query.list();
    	return infolist;
    }


  [1]: ./images/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20171017100917.png "微信截图_20171017100917.png"