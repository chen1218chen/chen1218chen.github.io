---
title: hibernate中Criteria实现复合条件查询
date: 2018-02-01 10:27:37
tags: [Hibernate,bootstrap table]
---
## 分页查询+多条件查询
分页查询+多条件查询，使用Criteria查询，逻辑清楚而且方便，避免了过多的if else 语句。以下代码中因为查询结果要进行分页，多以将每页数据和查询总数封装到map中传到前台，以供前台bootstrap-table插件使用

    public Map<String, List> dataByPageItem(int pageNow, int pageSize, String searchText, String flag,String jurisdiction, String severity) {
    		// TODO Auto-generated method stub
    		Map myMap = new HashMap<String, List>();
    		Integer total;
    		// 所有item数据分页
    		Session session = this.getSessionSelf();
    		Criteria crit = session.createCriteria(Item.class);
    		crit.setResultTransformer(Criteria.DISTINCT_ROOT_ENTITY);//解决查询处理的数据重复的问题
    		Conjunction conjunction = Restrictions.conjunction(); //and关系
    		Disjunction disjunction = Restrictions.disjunction(); //or关系
    		
    		if(!"9".equals(flag) && flag != ""){			
    			int flagInt = Integer.parseInt(flag);
    			Criterion flagCriterion = Restrictions.eq("flag",flagInt);
    			conjunction.add(flagCriterion);
    		}
    		if(jurisdiction!=""){
    			Criterion jurisdictionCrt = Restrictions.like("jurisdiction", jurisdiction);
    			conjunction.add(jurisdictionCrt);
    		}
    		if(severity!=""){
    			Criterion severityCrt = Restrictions.like("severity", severity);
    			conjunction.add(severityCrt);
    		}
    		if(searchText != ""){
    			Criterion address = Restrictions.like("address", searchText);
    			Criterion description = Restrictions.like("description", searchText);
    			Criterion dealName = Restrictions.like("dealName", searchText);
    			Criterion dataTime = Restrictions.like("dataTime", searchText);
    			disjunction.add(address);
    			disjunction.add(description);
    			disjunction.add(dealName);
    			disjunction.add(dataTime);
    			conjunction.add(disjunction);
    		}
    		
    		//排序
    		crit.add(conjunction).addOrder(Order.desc("dataTime"));  
    		// 增加查询条件 
    		Criteria critCount = crit;
    		total = critCount.list().size();
    		//分页
    		crit.setFirstResult((pageNow - 1) * pageSize);
    		crit.setMaxResults(pageSize);
    		List<Item> list = crit.list(); 
    		List<Integer> listTotal = new ArrayList<Integer>();
    		listTotal.add(total);
    		myMap.put("total", listTotal);//总数
    		myMap.put("rows", list);//每页数据
    		return myMap;
    	}