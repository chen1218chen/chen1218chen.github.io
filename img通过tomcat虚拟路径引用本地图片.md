---
title: img通过tomcat虚拟路径引用本地图片
date: 2018-04-17 14:26:55
tags: tomcat
---
由于web系统中有大量图片，为了避免tomcat文件越来越大，将图片文件夹移初tomcat目录下，放在本地磁盘中。
如果直接使用`<img src="D:\image.jpg">`引入，控制台会报无法找到图片的404错误。为解决这个问题，配置使用虚拟路径，让页面可以直接访问到服务器陌路以外的文件夹。



## 配置server.xml
在tomcat目录下，打开进入conf文件夹，编辑server.xml文件：添加如下配置：

    <Context path="/image" docBase="D:/ftp" reloadable="true" debug="0" crossContext="true" />
其中path为虚拟路径，  docBase是真实路径。
前台代码：

    <div><img src="/image/{{ image.imagePath }}" style="width:100%;height:100%" /></div>
    
![enter description here][1]


 
## server web modules
配置完server.xml文件之后，启动tomcat会发现在浏览器中依然无法访问到这个图片，原因是还需要在eclipse中修改server的Web Modules
双击tomcat server进入配置管理页面，点击Modules选项卡进入Web Modules页面点击第二个选项把相应的虚拟路径和真实路径也添加进去，下图所示：

![enter description here][2]


## 重启服务即可
重启服务即可看到

![enter description here][3]


  [1]: ./images/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20180417143344.png "微信截图_20180417143344.png"
  [2]: ./images/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20180417143515.png "微信截图_20180417143515.png"
  [3]: ./images/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20180417144056.png "微信截图_20180417144056.png"