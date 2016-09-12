---
title: 关于HTML5的一些零碎
date: 2016-09-05 14:25:19
tags: html
---
[TOC]

## 属性
### tabindex
tabindex 属性规定元素的 tab 键控制次序

    <input type="text" tabindex="1">
  
### defer 
现在html5的出现开始全面支持defer。defer 属性规定当页面已完成加载后，才会执行脚本。defer 属性仅适用于外部脚本（只有在使用 src 属性时）

### async
async 属性规定一旦脚本可用，则会异步执行