---
title: bootstrap file文件上传
tags: bootstrap
---

最近因为项目需要研究了下bootstrap fileinput的使用，来记录下这几天的使用心得吧。

> 前台html页面的代码
```
	<form role="form" id="importFile" method="post"
						enctype="multipart/form-data">
						<div class="row">
							<div class="col-md-3" >
								<input type="radio" name="excelType" class="radio" id="station" value="station"><label for="station">&nbsp;参数1</label>
							</div>
							<div class="col-md-3  ">
								<input type="radio" name="excelType" class="radio" id="line" value="line"><label for="line">&nbsp;参数2</label>
							</div>
							<div class="col-md-3 ">
								<input type="radio" name="excelType" class="radio" id="pipeline" value="pipeline"><label for="pipeline">&nbsp;参数3</label>
							</div>
							<div class="col-md-3 ">
								<input type="radio" name="excelType" class="radio" id="jdj" value="jdj"><label for="jdj">&nbsp;参数4</label>
							</div>
						</div>
						<input id="excelFile" name="excelFile" class="file-loading"
							type="file" multiple accept=".xls,.xlsx" data-min-file-count="1"
							data-show-preview="true"> <br>
					</form>
```

> js进行插件的初始化和一些参数的设置
```
$("#excelFile").fileinput({
					uploadUrl:"rest/import/importExcel",//上传的地址
					uploadAsync: true,
					language : "zh",//设置语言
					showCaption: true,//是否显示标题
					showUpload: true, //是否显示上传按钮
					browseClass: "btn btn-primary", //按钮样式 
					allowedFileExtensions: ["xls", "xlsx"], //接收的文件后缀
					maxFileCount: 10,//最大上传文件数限制
					uploadAsync: true,
					previewFileIcon: '<i class="glyphicon glyphicon-file"></i>',
					allowedPreviewTypes: null, 
					previewFileIconSettings: {
						'docx': '<i class="glyphicon glyphicon-file"></i>',
						'xlsx': '<i class="glyphicon glyphicon-file"></i>',
						'pptx': '<i class="glyphicon glyphicon-file"></i>',
						'jpg': '<i class="glyphicon glyphicon-picture"></i>',
						'pdf': '<i class="glyphicon glyphicon-file"></i>',
						'zip': '<i class="glyphicon glyphicon-file"></i>',
					},
					uploadExtraData: function() {
						var extraValue = null;
						var radios = document.getElementsByName('excelType');
						for(var i=0;i<radios.length;i++){
							if(radios[i].checked){
								extraValue = radios[i].value;
							}
						}
						return {"excelType": extraValue};
					}
				});
```
注意： uploadExtraData函数中只能用原生JS来取值，不能用JQuery来获取值，此参数用来向后台传递附加参数，以方便处理，最简单的写法是：

```
uploadExtraData: {
						"excelType": document.getElementByID('id')
					}
```
> 文件上传成功后的回调方法

```

	$("#excelFile").on("fileuploaded", function(event, data, previewId, index) {
		alert("上传成功!");
		$("#excelImport").modal("hide");
		//后台处理后返回的经纬度坐标json数组，
		var array = data.response;
		console.dir(array);
		//jquery循环取经纬度坐标
		$.each(array,function(index,latAndLon){
			var lon = latAndLon.lon;
			var lat = latAndLon.lat;
			var point =  new Point(lon, lat, map.spatialReference);
			var symbol = new esri.symbol.SimpleMarkerSymbol().setStyle(
				    SimpleMarkerSymbol.STYLE_CIRCLE).setColor(
				    new Color([255,255,0,0.5]));
			var attr = {"address": "addressName" };
			var infoTemplate = new esri.InfoTemplate("标题", "地址 :${address}");
			var graphic = new Graphic(point,symbol,attr,infoTemplate);
			map.graphics.add(graphic);
		});
		
	});
```

arcgis中点的定义的两种方法：

```
		var point =  new Point(lon, lat, new SpatialReference({ wkid: 4326 }));
		var point =  new Point(lon, lat, map.spatialReference);
```

>后台Java处理，使用common fileupload插件来实现，此处限制只能上传excel 文件

```
	public JSONArray importExcel(HttpServletRequest request,
			HttpServletResponse response) throws Exception {

		final String allowFileSuffix = "xls,xlsx";
		Subject subject = SecurityUtils.getSubject();
		String uname = (String) subject.getPrincipal();
		String basePath = "D:" + File.separator + uname;
		File tmpDir = new File(basePath);// 初始化上传文件的临时存放目录
		JSONArray jsonArry = new JSONArray();

		if (!tmpDir.exists()) {
			tmpDir.mkdirs();
		}

		// 检查输入请求是否为multipart表单数据。
		if (ServletFileUpload.isMultipartContent(request)) {
			DiskFileItemFactory dff = new DiskFileItemFactory();// 创建该对象
			dff.setRepository(tmpDir);// 指定上传文件的临时目录
			dff.setSizeThreshold(1024000);// 指定在内存中缓存数据大小,单位为byte
			ServletFileUpload sfu = new ServletFileUpload(dff);// 创建该对象
			// sfu.setFileSizeMax(5000000);//指定单个上传文件的最大尺寸
			sfu.setSizeMax(10000000);// 指定一次上传多个文件的总尺寸
			sfu.setHeaderEncoding("utf-8");
			String type = null;

			List<FileItem> fileItems = new ArrayList<FileItem>();
			try {
				fileItems = sfu.parseRequest(request);
			} catch (FileUploadException e1) {
				System.out.println("文件上传发生错误" + e1.getMessage());
			}
			String fullPath = null;
			String fileName = null;
			for (FileItem fileItem : fileItems) {

				// 判断该表单项是否是普通类型
				if (fileItem.isFormField()) {
					String name = fileItem.getFieldName();// 控件名
					String value = fileItem.getString();
					if (name.equals("excelType")) {
						type = value;
					}
				} else {
					String filePath = fileItem.getName();
					if (filePath == null || filePath.trim().length() == 0)
						continue;
					fileName = filePath.substring(filePath
							.lastIndexOf(File.separator) + 1);
					String extName = filePath.substring(filePath
							.lastIndexOf(".") + 1);
					fullPath = basePath + File.separator + fileName;
					if (allowFileSuffix.indexOf(extName) != -1) {
						try {
							fileItem.write(new File(fullPath));
						} catch (Exception e) {
							e.printStackTrace();
						}

					} else {
						throw new FileUploadException("文件格式不正确");
					}
				}
			}
			if (type.equals("station")) {
				jsonArry = readExcel(fullPath, fileName);
			} else if (type.equals("line")) {
				System.out.println("===============:line");
			} else if (type.equals("pipeline")) {
				System.out.println("===============:pipeline");
			} else if (type.equals("jdj")) {
				System.out.println("===============:jdj");
			}
		}
		return jsonArry;
	}
	
	// 判断文件类型
	public Workbook createWorkBook(InputStream is, String excelFileName)
			throws IOException {
		if (excelFileName.toLowerCase().endsWith("xls")) {
			return new HSSFWorkbook(is);
		}
		if (excelFileName.toLowerCase().endsWith("xlsx")) {
			return new XSSFWorkbook(is);
		}
		return null;
	}

	public JSONArray readExcel(String basePath, String fileName)
			throws FileNotFoundException, IOException {
		File file = new File(basePath);
		Workbook book = createWorkBook(new FileInputStream(file), fileName);
		JSONObject jsonObj = new JSONObject();
		JSONArray jsonArry = new JSONArray();
		Sheet sheet = book.getSheetAt(0);
		for (int i = 3; i < sheet.getLastRowNum(); i++) {
			Row row = sheet.getRow(i);
			String lon = row.getCell(2).getNumericCellValue() + "";
			String lat = row.getCell(3).getNumericCellValue() + "";
			jsonObj.put("lat", lat);// 纬度
			jsonObj.put("lon", lon);// 经度
			jsonArry.add(jsonObj);
		}
		System.out.println(jsonArry.toString());
		return jsonArry;
	}
```
基本流程终于走通啦，修改后记录一下跟大家分享，欢迎拍砖~~~