---
title: jquery类库
date: 2016-08-08 15:44:28
tags: jQuery
---
[TOC]
# jquery类库
## jQuery的setter和getter
### 获取和设置html属性-attr()
获取和设置html属性。与之相关的是removeAttr()用来移除选中元素的某个属性。

    # 获取a标签的href值
    $("a").attr("href");

    #设置src值
    $("#img").attr("src","images/a.png");

    #一次设置多个属性值
    $("#banner").attr({src:"banner.png",alt:"图片",width:"200px",heigth:"200px"});

    #连接在新窗口打开
    $("a").attr("target","_blank");

    #站内连接在本窗口，站外连接在新窗口打开
    $("a").attr("target",function(){
        if(this.host==location.host)
            return "_slef";
        else
            return "_blank"
    })
    或者
    $("a").attr(target:function(){
        ...//像这样传入函数
    })
由上例子可看出，设置参数的值时，可以传入函数。
### 获取设置css值-css()
获取设置css值，用法与attr()类似。
不能获取font,margin,padding等复合样式的值，只能取单个值，比如"font-size","margin-left"等。
### 获取设置CSS类
jQuery中提供了一下四种方法来获取和设置CSS类。

    addClass()
    removeClass()

    # 当元素没有某类时添加，反之删除
    toggleClass()

    # 用来判断某类是否存在，存在返回true
    hasClass() 

### 获取设置HTML表单值-val()
val()用来获取HTML表单元素的value属性，也可用来获取和设置复选框、单选按钮以及select元素的选中状态。

    #获取input值，select的选中值
    $("id").val();

    #获取选中的单选按钮的值
    $("input:radio[name=ship]:checked").val();
### 获取设置元素内容
text(),html()获取和设置元素的纯文本和html内容
### 获取设置元素的位置和大小
offset():获取元素位置，返回的对象带有left,top属性，用来表示X和Y坐标

    var node = $("div");//需要移动的元素
    var position = node.offset();//元素当前位置
    position.top +=100;//改变Y轴位置
    node.offset(position);//设置新位置
获取元素高度：

    #返回基本的宽度和高度,不含内外边距和边框
    width(),heigth()

    #返回包含内边距和边框的尺寸
    innerWidth(),innerHeight()

    #返回边框以内的尺寸，包括内边距，若向两个方法中的任意一个传入true值，它可以返回包括元素外边距的尺寸。
    outerWidth(),outerHeight()

    #左右外边距的和
    var margin = body.outerWidth(true)-body.outerWidth();
    # 左右边框的和
    var border = body.outerWidth()-body.innerWidth();
    #左右内边距的和
    var padding = body.innerWidth()-body.width();

### data()

    <div data-test="this is test"></div>
    $("div").data("test");//this is test


## jQuery选择器
除了常见的id选择器，类选择器，元素选择器之外还有以下选择器：
### 表单选择器

- **:input**
- **:radio**
- **:text**
- **:password**
- **:checkbox**
- **:submit**
- **:image**
- **:reset**
- **:button**
- **:file**
### 表单对象属性
- **:enabled**
- **:disabled** 匹配所有不可用元素
- **:checked** 匹配所有选中的被选中元素(复选框、单选框等，select中的option)，对于select元素来说，获取选中推荐使用 :selected
- **:selected** 匹配所有选中的option元素

。。。
后续待补充