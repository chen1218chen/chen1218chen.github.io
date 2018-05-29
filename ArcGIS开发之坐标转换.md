---
title: ArcGIS开发之geometryService的relation关系和查询
date: 2018-04-26 13:51:22
tags: arcgis
---
## 坐标转换
将原始坐标转换成wkid:32652坐标，并且使用回调将数据返回


    back.coordinateConvert = function(geometry,callback){
    	var geometryService = new GeometryService(baseURL+"/Utilities/Geometry/GeometryServer");			
    	 var params = new ProjectParameters();
    	  params.geometries = [geometry];
    	  params.outSR = new SpatialReference(32652);
    	  geometryService.project(params).then(function(resGeometry){
    		  callback(resGeometry);
    	  })
    }
    
    
## 数据查询
区域图层数据查询，并且使用回调将数据返回


    back.getReginData = function(callback){
      var query = new Query();
      query.where = "1=1";
      query.returnGeometry = true;
      query.outFields = ["*"];
      var queryTask = new QueryTask("http://localhost:6080/arcgis/rest/services/dt1124/MapServer/18");
      queryTask.execute(query,function(response){
    	  callback(response);
      })
    }
    
## relation
用于判断geometry之间的关系，如相交，包含，相切等

    back.relation = function(geometries1,geometries2){
        var geometryService = new GeometryService(baseURL+"Utilities/Geometry/GeometryServer");
        var relationParams = new RelationParameters();
        relationParams.geometries1 = geometries1;
        relationParams.geometries2 = geometries2;
        relationParams.relation = RelationParameters.SPATIAL_REL_INTERSECTION;
        var result = geometryService.relation(relationParams);
        return result;
    }
> SPATIAL_REL_INTERSECTION:相交。可选择不同的关系
> 返回值result是个Deferred对象，调用时：


    var relation = back.relation(resGeometry,geometryArray);
    relation.then(function(data){
        console.dir(data);
    })
结果如下图所示：

![enter description here][1]
表示存在两个相交关系：`geometries1[0]`与`geometries2[3]`相交，`geometries1[1]`与`geometries2[0]`相交

  [1]: ./images/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20180426135925.png "微信截图_20180426135925.png"