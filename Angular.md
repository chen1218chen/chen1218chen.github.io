---
title: Angular
date: 2016-06-07 16:07:01
tags: 
    - angular
---
# 初识Angular
angular是一种MVVM前端框架，解决了前后端代码的耦合性，可以分开开发部署；这样使得程序模块化高，可维护性高，灵活性高；可测试性，单元测试方便
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
        angular.element(document).ready(function() {
        	angular.bootstrap(document, [ 'myApp' ]);
        });
    });
注意：不能添加ng-app属性，采用手动启动angularjs应用