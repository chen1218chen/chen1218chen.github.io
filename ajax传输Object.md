---
title: ajax传输Object及@RequestBody的用法
date: 2017-01-11 14:27:19
tags: ajax
---
通过ajax向后台传输object对象。
## javascript

    $.ajax({
        url:"rest/publicgarden/edit",
        type:"post",
        cache:false,
        dataType:"json",
        contentType:"application/json; charset=utf-8",
        data:JSON.stringify(oldValue),
        success:function(result){
        	if (result)
        		alertConfirm("修改成功");
        	else
        		alertView("修改失败");
        }
    })
![oldValue][1]

### JSON.stringify()
以将任意的JavaScript值序列化成符合JSON语法的字符串。

    JSON.stringify({});                        // '{}'
    JSON.stringify(true);                      // 'true'
    JSON.stringify("foo");                     // '"foo"'
    JSON.stringify([1, "false", false]);       // '[1,"false",false]'
    JSON.stringify({ x: 5 });                  // '{"x":5}'
    
    JSON.stringify({x: 5, y: 6});              
    // '{"x":5,"y":6}' 或者 '{"y":6,"x":5}' 都可能
## java
### @RequestBody
@RequestBody接收的是一个Json对象的字符串，而不是一个Json对象。然而在ajax请求往往传的都是Json对象，后来发现用 JSON.stringify(data)的方式就能将对象变成字符串。同时ajax请求的时候也要指定dataType: "json",contentType:"application/json" 这样就可以轻易的将一个对象或者List传到Java端，使用@RequestBody即可绑定对象或者List.

    @RequestMapping(value = "/edit",method=RequestMethod.POST)
    @ResponseBody
    public boolean edit(@RequestBody Object value) {
        
        JSONObject jsonObj = JSONObject.fromObject(value);//将json字符串转换为json对象
        PublicGarden publicgarden = (PublicGarden) JSONObject.toBean(jsonObj,PublicGarden.class);//将建json对象转换为PublicGarden对象
        ipGardenServiceImpl.update(publicgarden);
        return true;
    }

    @RequestMapping(value = "saveUser", method = {RequestMethod.POST }}) 
    @ResponseBody  
    public void saveUser(@RequestBody List<User> users) { 
         userService.batchSave(users); 
    } 
    
[java与JSON相互转换][2]详情请点击这里查看。


  [1]: ./images/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20170111143952.png "微信截图_20170111143952.png"
  [2]: http://chen1218chen.github.io/2017/01/11/java%E4%B8%8EJSON%E5%AF%B9%E8%B1%A1%E7%9B%B8%E4%BA%92%E8%BD%AC%E6%8D%A2/