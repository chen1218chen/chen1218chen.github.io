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
        angular.element(document).ready(function() {
        	angular.bootstrap(document, [ 'myApp' ]);
        });
    });
注意：不能添加ng-app属性，采用手动启动angularjs应用
## 插件
###  1. angularAMD
 RequireJS+AngularJS
angularAMD is an utility that facilitates the use of RequireJS in AngularJS applications supporting on-demand loading of 3rd party modules such as angular-ui.

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

