---
title: ajax表单提交
date: 2017-10-31 14:51:24
tags: ajax
---

## 方法一

    $.ajax({
    	url : "rest/lost/saveLost",
    	type : "post",
    	cache : false,
    	dataType : 'json',
    	data : $(form).serializeArray(),
    	success : function(result) {
    		if (result)
    			alertConfirm("添加成功");
    		else
    			alertView("添加失败");
    	},
    });

### 后台java代码

    @RequestMapping(value="/saveLost",method = RequestMethod.POST)
    @ResponseBody
    public boolean saveLost(@RequestParam("goodName") String goodName,
    		@RequestParam("lostTime") String lostTime,
    		@RequestParam(value = "place", required = false) String place,
    		@RequestParam("descption") String descption,
    		@RequestParam(value = "telephone", required = true) String telephone) throws ParseException{
    	
    	Lost lost = new Lost();
    	lost.setDescption(descption);
    	lost.setGoodName(goodName);
    	Date time = (Date) Common.toDateStandard(lostTime);
    	lost.setLostTime(time);
    	lost.setTelephone(telephone);
    	lost.setPeople(telephone);
    	lost.setPlace(place);
    	lost.setStates("0");
    	try {
    		lostServiceImpl.insert(lost);
    	} catch (Exception e) {
    		// TODO Auto-generated catch block
    		e.printStackTrace();
    		return false;
    	}
    	return true;
    }
    
## 方法二

    function edit(oldValue){
    	$.ajax({
    		url:"rest/depart/edit",
    		type:"post",
    		cache:false,
    		dataType:"json",
    		contentType:"application/json; charset=utf-8",
    		data:JSON.stringify(oldValue),
    		success:function(result){
    			if (result){
    				alertView("修改成功");
    				query(oldValue.descption);
    			}			
    			else
    				alertView("修改失败");
    		}
    	})
    }

    @RequestMapping(value = "/edit",method=RequestMethod.POST)
    @ResponseBody
    public boolean edit(@RequestBody Object value) {
    	
    	JSONObject jsonObj = JSONObject.fromObject(value);//将json字符串转换为json对象
    	Department depart = (Department) JSONObject.toBean(jsonObj,Department.class);//将建json对象转换为PublicGarden对象
    	departServiceImpl.update(depart);
    	return true;
    }
直接上代码简单明了得意![enter description here][1]


  [1]: ./images/1509432811706.jpg "1509432811706.jpg"