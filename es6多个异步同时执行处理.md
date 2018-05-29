---
title: es6多个异步同时执行处理
date: 2018-04-26 14:07:41
tags: [JavaScript, ES6]
---

最近项目中数据处理时存在多次异步请求，需要等所有请求完成时在进行其他操作，为了节约时间提高效率，实现多个异步请求同时进行，在所有异步请求都结束后进行其他操作。
为了实现这一目的，使用es6的新特性，Promise对象来实现。

    function test1(){
        return new Promise((resole,reject)=>{
            ...
            resole(data);
        })
    }

     function test1(){
        return new Promise((resole,reject)=>{
            ...
            resole(data);
        })
    }

     function test3(){
        return new Promise((resole,reject)=>{
            ...
            resole(data);
        })
    }
使用Promise.all实现以上三个函数同时执行完时回调，values中则包含三个函数的返回值。代码实现如下：

    Promise.all(test1(),test2(),test3()).then(values=>{
        let test1Data = values[0];
        let test2Data = values[1];
        let test3Data = values[2];
        ...
    })