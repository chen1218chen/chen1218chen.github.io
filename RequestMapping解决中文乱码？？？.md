---
title: '@RequestMapping详解及解决中文乱码？？？'
date: 2018-03-19 10:57:20
tags: [springMVC]
---

## 乱码解决
最近接口对接，ajax请求rest接口，发现返回的中文数据全是？？？，如下图所示：

![enter description here][1]

后台控制台打印出来的数据却没有问题，那就有可能是传输过程中编码的问题
`@RequestMapping`添加`produces="application/json;charset=utf-8"`解决

    @RequestMapping(value = "/queryAll", method = { RequestMethod.POST }, produces="application/json;charset=utf-8")

## @RequestMapping参数
RequestMapping是一个用来处理请求地址映射的注解，可用于类或方法上。用于类上，表示类中的所有响应请求的方法都是以该地址作为父路径。

RequestMapping注解有六个属性，下面我们把她分成三类进行说明（下面有相应示例）。

1. value， method；
**value**：     指定请求的实际地址，指定的地址可以是URI Template 模式（后面将会说明）；
**method**：  指定请求的method类型， GET、POST、PUT、DELETE等；

2. consumes，produces
**consumes**： 指定处理请求的提交内容类型（Content-Type），例如application/json, text/html;
**produces**:    指定返回的内容类型，仅当request请求头中的(Accept)类型中包含该指定类型才返回；

3. params，headers
**params**： 指定request中必须包含某些参数值是，才让该方法处理。
**headers**： 指定request中必须包含某些指定的header值，才能让该方法处理请求。


  [1]: ./images/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20180319105857.png "微信图片_20180319105857.png"