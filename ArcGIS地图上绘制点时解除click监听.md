---
title: ArcGIS地图上绘制点时解除click监听
date: 2017-08-28 10:55:46
tags: [arcgis]
---

近日项目需求，开始着手学习基于arcgis地图的开发，一些地图上的基本操作则是第一步。
地图上绘制点时碰见一个一个小bug，不知道怎样解除click的监听，导致每点击一个地图就会出现一个圆点，网上资料不好找，不过最终解决了，Mark一下跟大家分享。(*^▽^*)

    back.drawPoint = function(){
    	var events = back.view.on("click",function(event){
    		console.log(event.mapPoint);
    		var lat =  event.mapPoint.latitude;
    		var lon =  event.mapPoint.longitude;
    		var point = new Point({
    			longitude: lon,
    		    latitude: lat
    		});
    		// Create a symbol for drawing the point
    		var markerSymbol = new SimpleMarkerSymbol({
    		color: [255, 0, 0],
            outline: { // autocasts as new SimpleLineSymbol()
              color: [0, 0, 0],
              width: 1
            }
          });

          var pointGraphic = new Graphic({
            geometry: point,
            symbol: markerSymbol
          });
          
          back.view.graphics.addMany([pointGraphic]);   
          events.remove();
    	})
    }

用的arcgis 4.4版本，back.view即mapview对象。