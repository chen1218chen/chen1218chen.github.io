---
title: ArcGIS入门
tags: arcgis
---

[TOC]

## 1. 初始化 ##
  
    function initMap(){
    	//地图初始化参数
    	map = new esri.Map("map", {
    		logo : false,//禁用logo
    		center: [107.55, 24.22],//地图中心点坐标
    		slider:false,
    		nav:false,
    		isZoomSlider:true,
    		autoResize : true,
    		smartNavigation : false,
    		force3DTransforms : true,
    		isKeyboardNavigation : false,
    		sliderStyle : "large",
    		showInfoWindowOnClick : true,
    		isDoubleClickZoom : false,
    	});
    	//比例尺
    	var scalebar = new esri.dijit.Scalebar({
    		map : map,
    		scalebarStyle : "ruler",
    		scalebarUnit : "metric",
    		attachTo : "top-left",
    	});
    	//缩略图
    	var overViewMap = new esri.dijit.OverviewMap({
    		map:map,
    		attachTo: "bottom-left",
    		visible:true,
    		opacity: .40,
    		width : 300,
    		height : 150,
    	});
    	overViewMap.startup();
    }

## 2. 指定地图中心点 ##

    //初始化点	
        var point =  new Point(107.55, 24.22, new SpatialReference({ wkid: 4326 }));
        //var point =  new Point(lon, lat, map.spatialReference);
        map.centerAndZoom(point,2);
        map.centerAt(point);

## 3. 加载地图 ##
index.jsp页面上需要添加

    String esriPath = request.getServerName() + ":" +
    request.getServerPort() + path + "/";
    
    <script type="text/javascript">
    	var arcgis_js_api_home = "<%=esriPath%>";
    	var dojoConfig = {
    		has : {
    			"dojo-firebug" : true
    		},
    		async : true,
    		paths : {
    			"js" : "/../js"
    		},
    	};
    </script>
再引入claro.css，esri.css，init.js，index.js。

index.js中添加

    require(["esri/map"
         ,"esri/layers/ArcGISDynamicMapServiceLayer"
         ,"esri/SpatialReference"
         ],function(){
    var map = new esri.Map("arcgisDiv",{
    	logo : false,
    	center: [107.55, 24.22],
    	slider:true,
    });
    var baseUrl = "http://192.168.1.139:6080/arcgis/rest/services/";
    var mapNames ={
    		"dt":"dt" //底图
    		,"dw":"dw"//电网
    	};
    var dtLayer = new esri.layers.ArcGISDynamicMapServiceLayer(baseUrl+mapNames.dt+"/MapServer",{id:"dt"});
    //	var layer1 = new esri.layers.ArcGISDynamicMapServiceLayer("http://localhost:6080/arcgis/rest/services/WGS_84/MapServer");
    var dwLayer = new esri.layers.ArcGISDynamicMapServiceLayer(baseUrl+mapNames.dw+"/MapServer",{id:"dw"});
    map.addLayers([dtLayer,dwLayer]);
    //	map.addLayer(layer);
    map.setZoom(8);
    });

## 4. infoWindow ##

    <script>
    require([
      "dojo/dom",
      "dojo/dom-construct",
      "esri/map", 
      "myModules/InfoWindow",
      "esri/layers/FeatureLayer",
      "esri/InfoTemplate",
      "dojo/string",
      "dojo/domReady!"
    ], function(
       dom,
       domConstruct,
       Map,
       InfoWindow,
       FeatureLayer,
       InfoTemplate,
       string
    ) {
      //create the custom info window specifying any input options
       var infoWindow = new  InfoWindow({
          domNode: domConstruct.create("div", null, dom.byId("mapDiv"))
       });
      var map = new Map("map", {
          basemap: "gray",
          center: [-98.57, 39.82],
          zoom: 4,
          infoWindow: infoWindow
        });

      var template = new PopupTemplate({
          title: "Boston Marathon 2013",
          description: "{Percent_Fi} of starters from {STATE_NAME} finished",
          fieldInfos: [{ //define field infos so we can specify an alias
            fieldName: "Number_Ent",
            label: "Entrants"
          },{
            fieldName: "Number_Sta",
            label: "Starters"
          },{
            fieldName: "Number_Fin",
            label: "Finishers"
          }]
        });

       var featureLayer = new FeatureLayer("http://services.arcgis.com/V6ZHFr6zdgNZuVG0/arcgis/rest/services/Boston_Marathon/FeatureServer/0",{
          mode: FeatureLayer.MODE_ONDEMAND,
          outFields: ["*"],
          infoTemplate:template
        });
        
        map.addLayer(featureLayer);  
      //resize the info window 
      map.infoWindow.resize(400, 200);
    });
    </script>


