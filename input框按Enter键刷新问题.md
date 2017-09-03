---
title: input框按Enter键刷新问题
date: 2017-09-03 12:55:18
tags: html
---

最近项目中遇到一个奇怪的问题，input text框实现的搜索功能，在输入查询条件后，一点击enter键页面就被刷新了，到一个一个组织页面刷新的好方法，在input标签中加入`onkeydown="if(event.keyCode==13){return false;}"`即可，进一步也可以将enter键改为搜索的提交，将`return false代码换成搜索提交即可`

    <input id="searchName" type="text" class="form-control" placeholder="输入信息" onkeydown="if(event.keyCode==13){return false;}">
