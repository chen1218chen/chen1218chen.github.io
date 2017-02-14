---
title: list<Object>的排序
date: 2017-02-14 11:40:24
tags: java
---

list集合的排序
当集合中存放的是Object复杂对象时，集成Comparable接口来实现排序。
例如：

    @Entity
    @Table(name = "guide")
    public class Guide implements java.io.Serializable,Comparable<Guide> {
        
        private Integer id;
        private Integer number;//排序序号
        ...(此处省略set，get方法)

        @Override
        public int compareTo(Guide o) {
        	int i = this.getNumber() - o.getNumber();// 先按照number排序
        	if (i == 0) {
        		return this.getId() - o.getId();// 如果number相等了再用id进行排序
        	}
        	return i;
        }
    
    }

用`Collections.sort(list);`直接排序

    @RequestMapping(value = "/queryAllDepart",method=RequestMethod.GET)
    @ResponseBody
    public JSONArray queryAllDepart(){

    	List<Department> list = departServiceImpl.queryAll();
    	Collections.sort(list);
    	JSONArray lostJSONList = JSONArray.fromObject(list, Common.getJsonConfig());
    	return lostJSONList;
    }