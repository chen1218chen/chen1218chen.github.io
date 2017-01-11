---
title: Java与JSON对象相互转换
date: 2017-01-11 15:00:48
tags: [java, JSON]
---

## json字符串——>json对象 JSONObject.fromObject()

    JSONObject jsonObj = JSONObject.fromObject(value);
rest中@RequestBody接收的就是一个Json对象的字符串，所以需要先转换为json对象
## json对象——>JavaBean对象 JSONObject.toBean()

    PublicGarden publicgarden = (PublicGarden) JSONObject.toBean(jsonObj,PublicGarden.class);