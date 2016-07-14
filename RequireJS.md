---
title: RequireJS
date: 2016-07-13 16:02:31
tags: RequireJS
---
[TOC]
# RequireJS
## 插件
1. RequireJS官方默认提供了一些插件，如：text，domReady，i18n。
其他详情请查看[RequireJS的插件列表][1]
2. [requirejs-plugins插件][2]

可通过bower安装

    bower install requirejs-plugins --save
## 配置文件
requireJS的配置常用文件config.js:

    requirejs.config({
        baseUrl : 'lib/',
        paths : {
        	app : '../js/app',
        	css : '../js/css.min',
        	custom: '../js/custom',
        	common: '../js/common',
        	domReady: 'domReady/domReady',
        	jquery : 'jquery/jquery.min',
        	async: 'requirejs-plugins/async',
        	react : 'react/react.min',
        	angular : 'angular/angular',
        	bootstrap : 'bootstrap/js/bootstrap.min',
        	bootstrapTable : 'bootstrap-table/js/bootstrap-table.min',
        	tableLanguage : 'bootstrap-table/locale/bootstrap-table-zh-CN.min',
        	tableExport:'bootstrap-table/extensions/tableExport',
        	bootstrapTableExport:'bootstrap-table/extensions/bootstrap-table-export.min',
        	confirm : 'jquery-confirm2/jquery-confirm.min',
        	'bootstrap-dialog' : 'bootstrap3-dialog/js/bootstrap-dialog.min',
        	BMap : 'http://api.map.baidu.com/api?v=2.0&ak=Ua9wlu6LbqmNKVbMEGtkgSxvljqZC5fc'
        },
        shim : {
        	jquery : {
        		init : function() {
        			return jquery.noConflict(true);
        		},
        		exports : 'jquery'
        	},
        	custom : {
        		deps: ['jquery']
        	},
        	common : {
        		deps: ['confirm', 'bootstrap-dialog']
        	},
        	index : {
        		deps: ['jquery']
        	},
        	angular : {
        		exports : 'angular'
        	},
        	
        	bootstrap : {
        		deps : [ 'jquery', 'css!../lib/bootstrap/css/bootstrap.min.css' ],
        		exports : "$.fn.popover"
        	},
        	tableLanguage : {
        		deps : [ 'bootstrapTable' ],
        		exports : '$.fn.bootstrapTable.defaults'
        	},
        	bootstrapTable : {
        		deps : ['bootstrap',
        		exports : '$.fn.bootstrapTable'
        	},
        	tableExport : {
        		deps: ['jquery'],
        		exports : '$.fn.extend'
        	},
        	bootstrapTableExport:{
        		deps:['bootstrapTable'],
        		exports : '$.fn.bootstrapTable.defaults'
        	},
        	confirm : {
        		deps : [ 'jquery','css!./jquery-confirm2/jquery-confirm.min.css' ]
        	},
        	'bootstrap-dialog' : {
        		deps : ['bootstrap', 'css!./bootstrap3-dialog/css/bootstrap-dialog.min.css' ]
        	},
        	'BMap': {
                deps: ['jquery'],
                exports: 'BMap'
            }
        	
        },
        //首先加载bootstrap
        deps: ['bootstrap'],
        //开发专用，阻止浏览器缓存
        urlArgs: "bust=" +  (new Date()).getTime(),
        //在放弃加载一个脚本之前等待的秒数。
        waitSeconds: 0
    });
常用参数解析如下：
### deps
配置首先加载的模块。

    deps: ['bootstrap'],
### urlArgs
开发中用来组织浏览器缓存

    urlArgs: "bust=" +  (new Date()).getTime(),
### waitSeconds
加载脚本的最长等待时间,设为0禁用等待超时。默认为7秒。
    
  [1]: https://github.com/requirejs/requirejs/wiki/Plugins
  [2]: https://github.com/millermedeiros/requirejs-plugins