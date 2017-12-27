---
title: hibernate之使用占位符进行模糊查询
date: 2017-09-29 15:31:27
tags: [Hibernate]
---
为防止sql注入攻击，使用hql语句进行查询时，不建议在hql语句中拼接字符串，而是使用占位符的方法来注入参数，如下代码所示：

> 占位符进行模糊查询


    @Override
    public List<Uploadinfo> searchInfos(String value) {
    	// TODO Auto-generated method stub
    	String hql = "from Uploadinfo where content like :value or telephone like :value";
    	Query query = itemDaoImpl.getQuery(hql);
    	query.setParameter("value", '%'+value+'%');
    	List<Uploadinfo> infolist = query.list();
    	return infolist;
    }
