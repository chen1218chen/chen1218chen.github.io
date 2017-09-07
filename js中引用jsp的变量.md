---
title: js中引用jsp的变量
date: 2017-09-07 13:44:07
tags: [JavaScript,JSP]
---

    <%
    	String path = request.getContextPath();
    	String basePath = request.getScheme() + "://"
    			+ request.getServerName() + ":" + request.getServerPort()
    			+ path + "/";
    %>

    <script type="text/javascript">
        var arcgis_js_api_home = "<%=arcgis_js_api_home%>";
        var basePath = "<%=basePath%>"; 
    </script>