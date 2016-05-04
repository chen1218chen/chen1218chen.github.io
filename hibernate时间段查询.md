---
title: hibernate时间段查询
date: 2016-05-04 15:55:02
tags: hibernate
---

## java
    
    @RequestMapping(value = "/searchDate", method = RequestMethod.POST)
	@ResponseBody
	public List<Uploadinfo> searchDate(@RequestParam("start") String start,@RequestParam("end") String end) {
		
		List<Uploadinfo> itemList = itemServiceImpl.dateRange(start,end);
		return itemList;
	}
	//使用HQL语句查询，预设字段
	public List<Uploadinfo> dateRange(String start, String end) {
		// TODO Auto-generated method stub
		
		Date beginDate = java.sql.Date.valueOf(start);
		Date endDate = java.sql.Date.valueOf(end);
		String hql = "from Uploadinfo info where info.dataTime <:endDate and info.dataTime >=:beginDate";  
		Query query = itemDaoImpl.getQuery(hql);
		query.setDate("beginDate",beginDate);   
		query.setDate("endDate",endDate);
		List<Uploadinfo> infolist = query.list();
		return infolist;
	}

重点在hql语句对时间的查询，由于数据库定义的是date类型，则需要将字符串类型转化为date类型
## js
前台使用[datarangepicker插件][1]
    
    function dateInit(){
    	$('#dateRange').daterangepicker({
    	    "ranges": {
    	        "Today": [
    	            "2016-05-04T05:40:05.879Z",
    	            "2016-05-04T05:40:05.879Z"
    	        ],
    	        "Yesterday": [
    	            "2016-05-03T05:40:05.879Z",
    	            "2016-05-03T05:40:05.879Z"
    	        ],
    	        "Last 7 Days": [
    	            "2016-04-28T05:40:05.879Z",
    	            "2016-05-04T05:40:05.879Z"
    	        ],
    	        "Last 30 Days": [
    	            "2016-04-05T05:40:05.879Z",
    	            "2016-05-04T05:40:05.879Z"
    	        ],
    	        "This Month": [
    	            "2016-04-30T16:00:00.000Z",
    	            "2016-05-31T15:59:59.999Z"
    	        ],
    	        "Last Month": [
    	            "2016-03-31T16:00:00.000Z",
    	            "2016-04-30T15:59:59.999Z"
    	        ]
    	    },
    	    "locale": {
    	        "format": "YYYY-MM-DD",
    	        "separator": "   ",
    	        "applyLabel": "确定",
    	        "cancelLabel": "取消",
    	        "fromLabel": "From",
    	        "toLabel": "To",
    	        "customRangeLabel": "Custom",
    	        "daysOfWeek": [
    	            "日",
    	            "一",
    	            "二",
    	            "三",
    	            "四",
    	            "五",
    	            "六"
    	        ],
    	        "monthNames": [
    	            "一月",
    	            "二月",
    	            "三月",
    	            "四月",
    	            "五月",
    	            "六月",
    	            "七月",
    	            "八月",
    	            "九月",
    	            "十月",
    	            "十一月",
    	            "十二月"
    	        ],
    	        "firstDay": 1
    	    },
    	    "alwaysShowCalendars": true,
    	    "startDate": "2016-05-04",
    	    "endDate": "2016-06-03"
    	}, function(start, end, label) {
    		searchDate(start.format('YYYY-MM-DD'),end.format('YYYY-MM-DD'));
    	});
    }
    
    function searchDate(start,end){
    	$.ajax({
    		url : "rest/Uploadinfo/searchDate",
    		type : "post",
    		dataType : 'json',
    		data:{
    			"start":start,
    			"end":end
    		},
    		success : function(data) {
    			tableShow(data);
    		}
    	});
    }


  [1]: http://www.daterangepicker.com/#examples