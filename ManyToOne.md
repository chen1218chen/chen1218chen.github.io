---
title: Hibernate ManyToOne, ManyToMany注解
date: 2016-03-31 15:02:55
tags: Hibernate
---
**hibernate配置相互关系的原则：**
- 在依赖方（many方）配置，单向依赖，这样可以避免出现循环。
- one是关系维护方，many是被维护方。在关系被维护端，即many端需要通过@JoinColumn建立外键列指向关系维护端的主键列。
    - manyTomany中@JoinColumn注解的都是在“主控方”，都是本表指向外表的外键名称。
    - 一对多的@JoinColumn注解在“被控方”，即一的一方，指的是外表中指向本表的外键名称。
    - 多对多中，joinColumns写的都是本表在中间表的外键名称，
  inverseJoinColumns写的是另一个表在中间表的外键名称。

- mappedBy
    - 用来定义双向关系，如果是单向关系，则不用定义。用来指定主控方。
- optional
    - 属性是定义该关联类是否必须存在。默认true，被维护方可以不存在。若为false则双方都必须存在，否则查询结果为null。

## manyToMany
user-role关系，单向只在user（依赖方）方配置

    @ManyToMany(cascade={CascadeType.MERGE}, fetch=FetchType.EAGER)
    @JoinTable(name = "user_to_role",   
    	joinColumns= { @JoinColumn(name = "user_id")},
    	inverseJoinColumns ={@JoinColumn(name = "role_id")  
    })
## ManyToOne
user-onganization关系，单向只在user（依赖方）方配置

    @ManyToMany(cascade={CascadeType.MERGE}, fetch=FetchType.EAGER)
    @JoinTable(name = "user_to_role",   
    	joinColumns= { @JoinColumn(name = "user_id")},
    	inverseJoinColumns ={@JoinColumn(name = "role_id")  
    })


## json死循环
>net.sf.json.JSONException: There is a cycle in the hierarchy!错误解决方案

    JsonConfig config = new JsonConfig();
    config.setExcludes(new String[]{"roles"});//除去roles属性
    JSONArray userJSONList2 = JSONArray.fromObject(dataList,config);
或者用以下方法解除循环

    @RequestMapping(value = "/queryAllOrgan", method = RequestMethod.GET)
	@ResponseBody
	public JSONArray queryAllOrgan() {
		
		JSONArray userJSONList;
		JsonConfig jsonConfig = new JsonConfig();
		jsonConfig.setIgnoreDefaultExcludes(false);
		jsonConfig.setCycleDetectionStrategy(CycleDetectionStrategy.LENIENT);
		jsonConfig.setExcludes(new String[] { "handler","hibernateLazyInitializer" });
		jsonConfig.setJsonPropertyFilter(new PropertyFilter() {
			public boolean apply(Object obj, String name, Object value) {
            //if(value instanceof Organization && name.equals("organ")){
			if(name.equals("user")){
					return true;
				} else {
					return false;
				}
			}
		});
		List<Organization> dataList = organServiceImpl.queryAll();
		userJSONList = JSONArray.fromObject(dataList,jsonConfig) ;
		return userJSONList;
	}

出现这种错误@ManyToOne 与@OneToMany是否配置正确，有没有配反

    //mappedBy：这个为manytoone中的对象名，这个不要变哦
    //	@OneToMany(cascade=CascadeType.ALL,fetch=FetchType.LAZY,mappedBy="user")
    //	@OneToMany(targetEntity = Roles.class, mappedBy = "user", cascade = { CascadeType.MERGE })
