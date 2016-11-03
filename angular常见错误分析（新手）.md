---
title: angular常见错误分析（新手篇）
date: 2016-11-02 11:17:56
tags: angular
---

新手入门angular碰见各种错误，总结一下跟大家分享，希望有用。
> [ng:areq] Argument 'DemoCtrl' is not a function, got undefined！

这往往是因为忘记定义controller或者是声明了多次module，多次声明module会导致前边的module定义信息被清空，所以程序就会找不到已定义的组件。

> Error: [$injector:unpr] 

unpr全称是Unknown Provider也就是说没有找到你注入的东西

## Module
angular.module('MyApp',[...])会创建一个新的Angular模块，然后把方括号（[...]）中的依赖列表加载进来；
而angular.module('MyApp')会使用由第一个调用定义的现有的模块。
所以，对于以下代码，你需要保证在整个应用中只会使用一次：

    angular.module('MyApp', [...]);
    
