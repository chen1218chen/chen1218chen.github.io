---
title: JS对象排序
date: 2018-04-17 14:07:05
tags: JavaScript
---
JS的对象排序的小技巧，使用sort()函数进行排序。

    console.dir(process);
    function compare(property){
        return function(a,b){
            var value1 = a[property];
            var value2 = b[property];
            return value1 - value2;
        }
    }
    var processSort = process.sort(compare('id'));
    console.dir(process);

控制台打印的输出如下图所示：
> 排序前

![排序前][1]
> 排序后

![排序后][2]


  [1]: ./images/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20180417145343.png "微信截图_20180417145343.png"
  [2]: ./images/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20180417142442.png "微信截图_20180417142442.png"