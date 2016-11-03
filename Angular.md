---
title: Angular
date: 2016-06-07 16:07:01
tags: angular
---

[TOC]

# 初识Angular
angular是一种MVVM前端框架，解决了前后端代码的耦合性，可以分开开发部署；这样使得程序模块化高，可维护性高，灵活性高；可测试性，单元测试方便
## 模块化
AngularJS 也是遵循 AMD 的。

    var app = angular.module('app', [
        'moduleA',
        'moduleB',
    ]);
    app.controller('MainCtrl', [
        '$scope',
         function ($scope) {
    }]);
# 核心概念
- 双向绑定：MVVM模型，view和model双向绑定，一方改变，另一方也会跟着变化。
- scope：view和controller之间的纽带，作为数据模型model，监听model的变化。每个angular-app只有一个rootScope，也存在childScope,每个childScope都会有parentScope和childScope，原型方式继承。
- controller：规范的angular中，controller中不出现任何DOM操作，controller仅仅是改变scope上的数据即可。
- view: angular会利用DOM树，经过complie再和scope数据结合，呈现给用户。

# angular应用入口
添加ng-app属性或者angular.bootstrap(element,['模块名'])启动。ng-app属性可以没有值，angular会默认创建一个。
ng-model指令的值挂在$scope对象上。

# angular + requirejs

    <div ng-controller="myCtrl">
    	<span>{{title}}</span>
    </div>

    requirejs.config({
        paths : {
            angular : 'angular/angular'
        },
        shim : {
            angular : {
                exports : 'angular'
            },
        }
    })
    define([ 'angular' ], function( angular) {
        var title = angular.module("myApp", []);
        title.controller('myCtrl', function($scope) {
        	$scope.title = "主页";
        });
        //手动启用angular
        angular.element(document).ready(function() {
        	angular.bootstrap(document, [ 'myApp' ]);
        });
    });
> 注意：不能添加ng-app属性，采用手动启动angularjs应用

## 异步加载注册controller／directive／filter／service
一般情况下我们会将项目所用到的controller／directive／filter／sercive预先加载完再初始化AngularJS模块，但是当项目比较复杂的情况下，应该是打开对应的界面才加载对应的controller等资源，但是AngularJS一旦初始化，之后加载的controller／directive／filter／sercive是不会自动注册到模块上的。用AngularJS + ui-router + RequireJS来构建项目应该是比较常见的，所以我就基于这个条件来看看如何解决这个问题。
参考：
[AngularJS + ui-router + RequireJS异步加载注册controller／directive／filter／service][1]

## Services
angular有两种方法来创建服务：
- 工厂 Factory
factory提供一些公共的方法函数，推荐抽象，重用factory。
- 服务 Service
service类似factory，会被实例化，可以保存数据，作为controller之间的通讯工具，比好好用。
### $http服务 
$http 是 AngularJS 中的一个核心服务，用于读取远程服务器的数据。

    var app = angular.module('myApp', []);
    app.controller('siteCtrl', function($scope, $http) {
      $http.get("http://www.runoob.com/try/angularjs/data/sites.php")
      .success(function (response) {$scope.names = response.sites;});
    });
### $timeout 服务
AngularJS $timeout 服务对应了 JS window.setTimeout 函数。
> 两秒后显示信息:


    var app = angular.module('myApp', []);
    app.controller('myCtrl', function($scope, $timeout) {
        $scope.myHeader = "Hello World!";
        $timeout(function () {
            $scope.myHeader = "How are you today?";
        }, 2000);
    });
### $interval 服务
> 每两秒显示信息:


    var app = angular.module('myApp', []);
    app.controller('myCtrl', function($scope, $interval) {
        $scope.theTime = new Date().toLocaleTimeString();
        $interval(function () {
            $scope.theTime = new Date().toLocaleTimeString();
        }, 1000);
    });
### 自定义服务
> 创建一个名为test的服务


    app.service('test', function() {
        this.myFunc = function (x) {
            return x.toString(16);
        }
    });
> 控制器myCtrl中调用


    app.controller('myCtrl', function($scope, test) {
        $scope.hex = test.myFunc(255);
    });
> 过滤器myFormat中的使用


    app.filter('myFormat',['hexafy', function(hexafy) {
        return function(x) {
            return hexafy.myFunc(x);
        };
    }]);
## Directive指令
扩展html语言，类似于React的组件

    var app = angular.module("myApp", []);
    app.directive("runoobDirective", function() {
        return {
            template : "<h1>自定义指令!</h1>"
        };
    });
    //以下两种方式调用都可以
    <runoob-directive></runoob-directive>
    <div runoob-directive></div>

类名调用

    app.directive("runoobDirective", function() {
        return {
            restrict : "C",
            template : "<h1>自定义指令!</h1>"
        };
    });
    <div class="runoob-directive"></div>

注释调用

    app.directive("runoobDirective", function() {
        return {
            restrict : "M",
            replace : true,
            template : "<h1>自定义指令!</h1>"
        };
    })
> 注意: 需要在该实例添加 replace 属性， 否则评论是不可见的。
注意: 必须设置 restrict 的值为 "M" 才能通过注释来调用指令。

### restrict
restrict 值可以是以下几种:

 - E 作为元素名使用 
 - A 作为属性使用 
 - C 作为类名使用 
 - M 作为注释使用

## 插件
###  1. angularAMD
 RequireJS+AngularJS
angularAMD is an utility that facilitates the use of RequireJS in AngularJS applications supporting on-demand loading of 3rd party modules such as angular-ui.
angularAMD是作者@ marcoslin使用 RequireJS ＋ AngularJS开发的前端mvvm框架,因此你可以使用它快速创建一款Web App.它特别适合快速开发SPA应用。

###  2. angularUI
 AngularJS 的UI增强指令集。包含的模块有：      
    UI-Utils     
    UI-Modules     
    UI-Alias     
    UI-Bootstrap     
    NG-Grid     
    UI-Router     
    IDE Plugins     
    GSoC

#### **UI-Router与ngRoute**
ngRoute是用AngularJS框架的核心部分
ui-router是一个社区库、第三方模块，它是用来提高完善ngroute路由功能的，功能非常强大。它支持一切正常ngroute也可以做许多额外的功能。
###  3. Angular UI Bootstrap
 UI Bootstrap 是由 AngularJS UI 团队编写的纯 AngularJS 实现的 Bootstrap 组件。
###  4. Mobile Angular UI
 
Mobile Angular UI是一个类似jQuery Mobile的移动UI框架


  [1]: http://www.tuicool.com/articles/IJNFNnf