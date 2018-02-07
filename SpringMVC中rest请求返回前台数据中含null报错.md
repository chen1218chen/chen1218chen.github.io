---
title: SpringMVC中rest请求返回前台数据中含null报错
date: 2018-01-11 14:33:03
tags: [springMVC,Hibernate]
---
最近SpringMVC框架的项目中频繁使用hibernate级联关系，如@OneToOne，@ManyToOne等，每次前台ajax发送rest请求总是后台报错，后台打印错误信息如下：

    org.springframework.http.converter.HttpMessageNotWritableException Could not write JSON: Object is null (through reference chain: net.sf.json.JSONObject["rows"]->net.sf.json.JSONArray[1]->net.sf.json.JSONObject["taskId"]->net.sf.json.JSONNull["empty"]); nested exception is com.fasterxml.jackson.databind.JsonMappingException: Object is null (through reference chain: net.sf.json.JSONObject["rows"]->net.sf.json.JSONArray[1]->net.sf.json.JSONObject["taskId"]->net.sf.json.JSONNull["empty"])] with root cause net.sf.json.JSONException: Object is null

应该是在Hibernate查询出数据实体类使用jackson序列化为json时报的错，因为实体数据的外键taskID为空
## @JsonInclude(Include.NON_NULL) 
1. 在实体上设置 `@JsonInclude(Include.NON_NULL)` 

将该标记放在属性上，如果该属性为NULL则不参与序列化 
如果放在类上边,那对这个类的全部属性起作用 

    Include.Include.ALWAYS 默认 
    Include.NON_DEFAULT 属性为默认值不序列化 
    Include.NON_EMPTY 属性为 空（“”） 或者为 NULL 都不序列化 
    Include.NON_NULL 属性为NULL 不序列化 


2. 代码上


    ObjectMapper mapper = new ObjectMapper();
    mapper.setSerializationInclusion(Include.NON_NULL);  

通过该方法对mapper对象进行设置，所有序列化的对象都将按改规则进行系列化 

    User user = new User(1,"",null); 
    String outJson = mapper.writeValueAsString(user); 
    System.out.println(outJson);

注意：只对实体VO起作用，Map List不起作用



参考：
[jackson 实体转json 为NULL或者为空不参加序列化][1]


  [1]: http://www.cnblogs.com/yangy608/p/3936848.html