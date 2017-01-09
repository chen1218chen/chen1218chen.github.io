---
title: bootstrap-fileinput之文件上传
date: 2016-09-28 11:02:45
tags: [Spring, MultipartFile, fileInput]
---

[TOC]


此功能实现使用bootstrap-fileinput(fileinput.min.js)插件,主页地址：

1. [bootstrap-fileinput Github][1]
2. [主页][2]
## MultipartFile
Spring使用MultipartFile来进行多文件上传，需要配置一个multipartResolver的bean

    <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver" />

    @RequestMapping(value="/parseAttr.json", method = RequestMethod.POST)
    public ModelMap parseAttr(@RequestParam("file") MultipartFile uFile) {
         ModelMap result = new ModelMap();
         ...
         result.put("keys",set);
         return result;
    }
## 常用方法

    MultipartFile file = new MultipartFile();   
    file.getOriginalFilename():获取上传文件的原名
    file.getInputStream():获取文件流



## form表单
分两类上传：办事指南、典型案例。


    <form role="form" class="form-horizontal" id="importFile" method="post" accept-charset="utf-8" enctype="multipart/form-data">
        <div class="form-group">
        	<label class="col-lg-2 control-label" for="descption">文章类型</label>
        	<div class="col-lg-9">
        		<select name="descption" id="descption" style="padding: 6px 12px;line-height: 1.42857143;    border: 1px solid #ccc;    border-radius: 4px;">
        			<option value="1">办事指南</option>
        			<option value="2">典型案例</option>
        		</select>
        	</div>
        </div>
        <div class="form-group">
        	<label class="col-lg-2 control-label" for="fileInput">文章上传</label>
        	<div class="col-md-9">
        
        		<input id="fileInput" name="fileInput" type="file" multiple
        			class="file-loading">
        	</div>
        </div>
        
        <hr />
        <div class="btn-group" style="margin-left: 160px;margin-right:5px">
        	<button type="button" class="btn btn-success" id="submitOK">
        		<i class="glyphicon glyphicon-check">提交</i>
        	</button>
        	<button type="button" class="btn btn-danger" data-dismiss="modal"
        		id="btn-close">
        		<i class="glyphicon glyphicon-remove">关闭</i>
        	</button>
        </div>
    </form> 

## js代码

    $("#fileInput").fileinput({
    	uploadUrl:"#",//上传的地址
    	uploadAsync: false,
    	language : "zh",//设置语言
    	showCaption: true,//是否显示标题
    	showUpload: false, //是否显示上传按钮
    	browseClass: "btn btn-primary", //按钮样式 
    	allowedFileExtensions: ["html","htm"], //接收的文件后缀
    	maxFileCount: 1,//最大上传文件数限制
    	maxFileSize: 1000,//最大上传文件大小
    	uploadAsync: true,
    	previewFileIcon: '<i class="glyphicon glyphicon-file"></i>',
    	allowedPreviewTypes: null, 
    	previewFileIconSettings: {
            'doc': '<i class="fa fa-file-word-o text-primary"></i>',
            'xls': '<i class="fa fa-file-excel-o text-success"></i>',
            'ppt': '<i class="fa fa-file-powerpoint-o text-danger"></i>',
            'jpg': '<i class="fa fa-file-photo-o text-warning"></i>',
            'pdf': '<i class="fa fa-file-pdf-o text-danger"></i>',
            'zip': '<i class="fa fa-file-archive-o text-muted"></i>',
            'htm': '<i class="fa fa-file-code-o text-info"></i>',
            'txt': '<i class="fa fa-file-text-o text-info"></i>',
            'mov': '<i class="fa fa-file-movie-o text-warning"></i>',
            'mp3': '<i class="fa fa-file-audio-o text-warning"></i>',
        },
        previewFileExtSettings: {
            'doc': function(ext) {
                return ext.match(/(doc|docx)$/i);
            },
            'xls': function(ext) {
                return ext.match(/(xls|xlsx)$/i);
            },
            'ppt': function(ext) {
                return ext.match(/(ppt|pptx)$/i);
            },
            'zip': function(ext) {
                return ext.match(/(zip|rar|tar|gzip|gz|7z)$/i);
            },
            'htm': function(ext) {
                return ext.match(/(php|js|css|htm|html)$/i);
            },
            'txt': function(ext) {
                return ext.match(/(txt|ini|md)$/i);
            },
            'mov': function(ext) {
                return ext.match(/(avi|mpg|mkv|mov|mp4|3gp|webm|wmv)$/i);
            },
            'mp3': function(ext) {
                return ext.match(/(mp3|wav)$/i);
            },
        }
    });

上传文件可附加参数，通过uploadExtraData参数来实现。

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

上传成功的回调函数：

     $("#fileInput").on("fileuploaded", function(event, data, previewId, index) {
    	alertView("上传成功!");
    }); 
    
上传失败的回调函数：

    $('#excelFile').on('fileuploaderror', function(event, data, previewId, index) {
        alertView("上传文件不正确!");
    });
js提交带文件表单

     $("#submitOK").click(function() {
    	$.ajax({
            cache: true,
            type: "POST",
            url: "rest/guide/importfile",
            data: new FormData($('#importFile')[0]),// 你的formid
            processData: false,
            contentType: false,
            error: function(request) {
                alert("上传失败");
            },
            success: function(data) {
            	if(data){            		
            		alert("上传成功");
            	}
            	else {
            		alert("上传失败");
            	}
            }
        });
    }); 

需要注意的以下几点：

- processData设置为false。因为data值是FormData对象，不需要对数据做处理。
- <form>标签添加enctype="multipart/form-data"属性。
- cache设置为false，上传文件不需要缓存。
- contentType设置为false。因为是由<form>表单构造的FormData对象，且已经声明了属性enctype="multipart/form-data"，所以这里设置为false。
- 上传后，服务器端代码需要使用从查询参数名为file获取文件输入流对象，因为input中声明的是name="file"。

## 上传文件后台

    @RequestMapping(value = "/importfile", method = RequestMethod.POST)
    @ResponseBody
    public boolean importfile(HttpServletRequest request, HttpServletResponse response) throws Exception {
    	request.setCharacterEncoding("utf-8");
    	final String allowFileSuffix = "html,htm";
    	String basePath = request.getSession().getServletContext().getRealPath("/");
    	String realpath = null;
        
    	// 检查输入请求是否为multipart表单数据。
    	if (ServletFileUpload.isMultipartContent(request)) {
    		DiskFileItemFactory dff = new DiskFileItemFactory();//获得磁盘文件条目工厂  
    		dff.setSizeThreshold(1024000);// 指定在内存中缓存数据大小,单位为byte
    		ServletFileUpload upload = new ServletFileUpload(dff);// 创建该对象
    		upload.setFileSizeMax(5000000);//指定单个上传文件的最大尺寸
    		upload.setSizeMax(10000000);// 指定一次上传多个文件的总尺寸
    		upload.setHeaderEncoding("utf-8");
    		List<FileItem> fileItems = new ArrayList<FileItem>();
    		try {
    			fileItems = upload.parseRequest(request);
    		} catch (FileUploadException e1) {
    			logger.error("文件上传发生错误" + e1.getMessage());
    			return false;
    		}
    		String fullPath = null;
    		String fileName = null;
    		String fullName=null;
    		String type="";
    		for (FileItem fileItem : fileItems) {

    			// 判断该表单项是否是普通类型
    			if (fileItem.isFormField()) {
    				String name = fileItem.getFieldName();// 控件名
    				String value = fileItem.getString(); //控件值		
    				if (name.equals("descption")) {
    					if(value.equals("1")){
    						type="guide";//办事指南
    					}
    					else if(value.equals("2")){
    						type="case";//典型案例
    					}
    					realpath=basePath+type;
    					File tmpDir = new File(realpath);// 初始化上传文件的临时存放目录
    					if (!tmpDir.exists()) {
    						tmpDir.mkdirs();
    					}
    					dff.setRepository(tmpDir);// 指定上传文件的临时目录

    				}
    			} else {
    				String filePath = fileItem.getName();
    				if (filePath == null || filePath.trim().length() == 0)
    					continue;
    				fullName = filePath.substring(filePath.lastIndexOf(File.separator) + 1);
    				fileName = filePath.substring(0,filePath.lastIndexOf("."));
    				
    				String extName = filePath.substring(filePath.lastIndexOf(".") + 1);
    				fullPath = realpath + File.separator + fullName;
    				if (allowFileSuffix.indexOf(extName) != -1) {
    					try {
    						fileItem.write(new File(fullPath));
    						//上传文件地址存入数据库
    						Guide guide = new Guide();
    						guide.setName(fileName);
    						guide.setUrl("../"+type +"/" + fullName);
    						if(type.equals("guide")){								
    							guide.setType(1);
    						}
    						else if(type.equals("case")){
    							guide.setType(2);
    						} 
    						guideServiceImpl.insert(guide);
    					} catch (Exception e) {
    						logger.error("文件写入Guide表出错");
    						return false;
    					}
    				} else {
    					throw new FileUploadException("文件格式不正确");
    				}
    			}
    		}
    	}
    	return true;
    }


  [1]: https://github.com/kartik-v/bootstrap-fileinput
  [2]: http://plugins.krajee.com/file-input