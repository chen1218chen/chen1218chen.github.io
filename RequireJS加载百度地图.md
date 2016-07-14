---
title: RequireJS模块化开发百度地图
date: 2016-07-14 10:58:16
tags: RequireJS
---
[TOC]
## 加载百度地图
因为百度地图只能CDN加载，所以paths中直接配置CDN路径
### 安装async插件
requirejs-plugins中包含async，[点击这里进行下载][1]

可通过bower安装

    bower install requirejs-plugins --save

    require.config({
        paths: {
            async: 'requirejs-plugins/async',
            'BMap' : 'http://api.map.baidu.com/api?v=2.0&ak=Ua9wlu6LbqmNKVbMEGtkgSxvljqZC5fc'
        },
        shim: {
            'BMap': {
            deps: ['jquery'],
            exports: 'BMap'
        }
        }
    }

    //调用百度地图
    define(['jquery','async!BMap'], function(){
        var map = new BMap.Map("map");
        var point = new BMap.Point(108.95344, 34.265664); // 创建点坐标
        map.centerAndZoom(point, 13); // 初始化地图，设置中心点坐标和地图级别
        map.centerAndZoom("西安", 13);
    });
这里注意，不能写成：

    define(['jquery','async!BMap'], function(jquery,BMap){
        ...
    });
否则报错，BMap is undefined
![enter description here][2]

## 分析
至今为止，百度地图JS API暂时还没有很好地支持RequireJS，API并没有对AMD做特殊设置。暂不支持AMD或者CMD的加载方式。



  [1]: https://github.com/millermedeiros/requirejs-plugins
  [2]: ./images/Image%201.png "Image 1.png"