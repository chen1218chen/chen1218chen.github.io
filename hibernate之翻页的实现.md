---
title: hibernate之翻页的实现
date: 2016-06-13 10:46:36
tags: hibernate, java
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