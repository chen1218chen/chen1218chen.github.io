---
title: angular之Service vs Factory
date: 2016-11-04 09:32:40
tags: angular
---

angular有两种方法来创建服务：
- 工厂 Factory
factory提供一些公共的方法函数，推荐抽象，重用factory。
- 服务 Service
service类似factory，会被实例化，可以保存数据，作为controller之间的通讯工具，比好好用。

在Angular源码中的定义：

    function factory(name, factoryFn) {
        return provider(name, { $get: factoryFn });
    }
    function service(name, constructor) {
        return factory(name, ['$injector', function($injector) {
          return $injector.instantiate(constructor);
        }]);
    }
下面的例子展示了service和factory做了同样一件事情：

    var app = angular.module('app',[]);
    app.service('helloWorldService', function(){
        this.hello = function() {
            return "Hello World";
        };
    });
    app.factory('helloWorldFactory', function(){
        return {
            hello: function() {
                return "Hello World";
            }
        }
    });
当helloWorldService或helloWorldFactory被注入到控制器中，它们都有一个hello方法，返回”hello world”。service的构造函数在声明时被实例化了一次，同时factory对象在每一次被注入时传递，但是仍然只有一个factory实例。**所有的providers都是单例。**
既然能做相同的事，为什么需要两种不同的风格呢？相对于service，factory提供了更多的灵活性，因为它可以返回函数，这些函数之后可以被新建出来。这迎合了面向对象编程中工厂模式的概念，工厂可以是一个能够创建其他对象的对象。

    app.factory('helloFactory', function() {
        return function(name) {
            this.name = name;
            this.hello = function() {
                return "Hello " + this.name;
            };
        };
    });
以下是一个控制器示例，使用了service和两个factory，helloFactory返回了一个函数，当新建对象时会设置name的值。

    app.controller('helloCtrl', function($scope, helloWorldService, helloWorldFactory, helloFactory) {
        init = function() {
          helloWorldService.hello(); //'Hello World'
          helloWorldFactory.hello(); //'Hello World'
          new helloFactory('Readers').hello() //'Hello Readers'
        }
        init();
    });
在初学时，最好只使用service。

Factory在设计一个包含很多私有方法的类时也很有用：

    app.factory('privateFactory', function(){
        var privateFunc = function(name) {
            return name.split("").reverse().join(""); //reverses the name
        };
        return {
            hello: function(name){
              return "Hello " + privateFunc(name);
            }
        };
    });

参考：
1. [AngularJS 开发中常犯的10个错误][1]


  [1]: http://www.yyyweb.com/3019.html