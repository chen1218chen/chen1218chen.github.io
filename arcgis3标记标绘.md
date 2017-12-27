---
title: arcgis3标记标绘
date: 2017-11-10 15:31:45
tags: arcgis
---
arcgis3.22算时当前最新的3版本的API了，项目需要，整理一下简单的标记标绘功能，以后就直接`crtl+C,ctrl+V`了。

代码符合AMD规范，方便模块化开发调用。
## polygon多边形

    back.drawPolygon = function(){
    	  toolbar = new Draw(ArcGIS.mainMap);
    	  toolbar.on("draw-end", addPolygon);
    	  toolbar.activate("polygon");
    }

    function addPolygon(evt){
    	 toolbar.deactivate();
    	 var symbol = new SimpleFillSymbol(  
    	            SimpleFillSymbol.STYLE_SOLID,  
    	            new SimpleLineSymbol(  
    	              SimpleLineSymbol.STYLE_SOLID,  
    	              new Color([255,0,0,0.65]), 2  
    	            ),  
    	            new Color([0,0,0,0.35])  
    	          ); 
    	 ArcGIS.mainMap.graphics.add(new Graphic(evt.geometry, symbol));
    }

>polygon图片填充

    var fillSymbol = new PictureFillSymbol(
          "images/mangrove.png", new SimpleLineSymbol(
            SimpleLineSymbol.STYLE_SOLID,
            new Color('#000'), 1), 42,42);
>polygon半透明，简单颜色填充

     var symbol = new SimpleFillSymbol(  
            SimpleFillSymbol.STYLE_SOLID,  
            new SimpleLineSymbol(  
              SimpleLineSymbol.STYLE_SOLID,  
              new Color([255,0,0,0.65]), 2  
            ),  
            new Color([0,0,0,0.35])  
          );
## Circle

    back.drawArea = function() {
    	  toolbar = new Draw(ArcGIS.mainMap);
    	  toolbar.on("draw-end", addCircle);
    	  toolbar.activate("circle");
    }

    function addCircle(evt){
      toolbar.deactivate();
     var symbol = new SimpleFillSymbol(  
            SimpleFillSymbol.STYLE_SOLID,  
            new SimpleLineSymbol(  
              SimpleLineSymbol.STYLE_SOLID,  
              new Color([255,0,0,0.65]), 2  
            ),  
            new Color([0,0,0,0.35])  
          );
      ArcGIS.mainMap.graphics.add(new Graphic(evt.geometry, symbol));
    }
## polyline折线

    back.drawDistance = function() {
    	tb = new Draw(ArcGIS.mainMap);
    	tb.on("draw-end", addPolyline);
    	ArcGIS.mainMap.disableMapNavigation();
    	tb.activate("polyline");
    }

    function addPolyline(evt) {
    	tb.deactivate();
    	ArcGIS.mainMap.enableMapNavigation();
    	var lineSymbol = new CartographicLineSymbol(
    			CartographicLineSymbol.STYLE_SOLID, new Color([ 255, 0, 0 ]),
    			3, CartographicLineSymbol.CAP_ROUND,
    			CartographicLineSymbol.JOIN_MITER, 2);
    	ArcGIS.mainMap.graphics.add(new Graphic(evt.geometry, lineSymbol));
    }
## point点

    back.drawPoint = function(url) {
    	var events = ArcGIS.mainMap.on("click", function(e) {
    		var pt = new esri.geometry.Point(e.mapPoint.x, e.mapPoint.y,
    				ArcGIS.mainMap.spatialReference);
    		var pictureMarkerSymbol = new PictureMarkerSymbol(url, 31, 31);
    		var attr = {
    			"name" : "1",
    			"remark" : "1"
    		};
    		graphic = new esri.Graphic(pt, pictureMarkerSymbol, attr, null);
    		ArcGIS.mainMap.graphics.add(graphic);
    		events.remove();
    	});
    }
    
