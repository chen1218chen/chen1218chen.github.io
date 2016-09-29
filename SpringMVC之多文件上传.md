---
title: SpringMVC之多文件上传
date: 2016-09-28 11:02:45
tags: [Spring, MultipartFile]
---
[TOC]

## MultipartFile
Spring使用MultipartFile来进行多文件上传，需要配置一个multipartResolver的bean

    <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver" />

    @RequestMapping(value="/parseAttr.json", method = RequestMethod.POST)
    public ModelMap parseAttr(@RequestParam("file") MultipartFile uFile) {
         ModelMap result = new ModelMap();
         ...
         result.put("keys",set);
         return result;
    }
## 常用方法

    MultipartFile file = new MultipartFile();   
file.getOriginalFilename():获取上传文件的原名
file.getInputStream():获取文件流