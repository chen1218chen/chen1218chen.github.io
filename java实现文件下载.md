---
title: java实现文件下载
date: 2017-03-03 11:56:48
tags: java
---
根据项目需求实现word文档下载功能，这是java的后台实现，前台调用采用ajax方式，参数是文件名


    import java.io.File;
    import java.io.FileInputStream;
    import java.io.IOException;
    import java.io.OutputStream;
    import java.net.URLEncoder;
    import javax.servlet.http.HttpServletRequest;
    import javax.servlet.http.HttpServletResponse;
    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;
    import org.springframework.stereotype.Controller;
    import org.springframework.web.bind.annotation.PathVariable;
    import org.springframework.web.bind.annotation.RequestMapping;
    import org.springframework.web.bind.annotation.RequestMethod;
    import org.springframework.web.bind.annotation.ResponseBody;

    @Controller
    @RequestMapping("laws")
    public class LawsRest {
    	private static Logger logger = LoggerFactory.getLogger(LawsRest.class);

    	@RequestMapping(value = "/downloadFile/{value}",method=RequestMethod.GET)
    	@ResponseBody
    	public void downloadFile(@PathVariable("value") String value, HttpServletRequest request,
    			HttpServletResponse response) throws IOException {

    		String directory = request.getSession().getServletContext().getRealPath("/");
    		String fileName = value+".doc";
    		String filePath = directory + "laws\\" + fileName;
    	
    		//设置向浏览器端传送的文件格式
    		//判断文件是否存在
    		File file=new File(filePath);
    		if(!file.exists()){
    			fileName = value+".docx";
    			filePath = directory + "laws\\" + fileName;
    		}
    		logger.info(value);
    		logger.info(filePath);
            response.setContentType("application/msword");  
    		response.setHeader("content-disposition", "attachment;filename=" + URLEncoder.encode(fileName, "UTF-8"));
    		FileInputStream in = new FileInputStream(filePath);
    		// 创建输出流
    		OutputStream out = response.getOutputStream();
    		// 创建缓冲区
    		byte buffer[] = new byte[1024];
    		int len = 0;
    		// 循环将输入流中的内容读取到缓冲区当中
    		while ((len = in.read(buffer)) > 0) {
    			// 输出缓冲区的内容到浏览器,实现文件下载
    			out.write(buffer, 0, len);
    		}
    		out.flush();
    		
    		in.close();
    		out.close();
    	}
    }
