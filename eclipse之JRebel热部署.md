---
title: eclipse之JRebel热部署
date: 2016-06-21 10:57:15
tags: eclipse, JRebel
---

每次修改java文件都需要重启tomcat，最近实在是不堪其扰，果断决定再次折腾一下JRebel来实现热启动，从eclipse marketplace下载的这个插件都是14天的试用期，试用期过了之后就不能在使用了，今天重新捣鼓了一下，终于激活了，以后妈妈再也不用担心我不停的重启tomcat了。
1. Help > Install New Software中输入
http://update.zeroturnaround.com/update-site-archive/update-site-6.4.2.RELEASE/。
![enter description here][1]
2. 点击next进入下一步，根据提示一步步安装即可。
3. 安装成功后help里面就可以看见JRebel Configuration和JRebel Activation
![enter description here][2]
4. 激活
到[这里][3]下载破解文件到本地。解压，把压缩包里的：jrebel.jar和jreble.lic两个文件，覆盖到Eclipse安装目录下plugins文件夹下的org.zeroturnaround.eclipse.embedder_6.2.2.RELEASE-201507291337文件夹下所有包含jrebel.jar的子文件夹中。
加载license文件，然后重启Eclipse。即可看见显示JRebel activated。
![enter description here][4]

重启后，查看Eclipse>Window>Preferences会发现，多了一个JRebel的目录。
![enter description here][5]
点击JRebel Configuration即可看见：
![enter description here][6]
激活成功。


  [1]: ./images/Image%201.png "Image 1.png"
  [2]: ./images/Image%202.png "Image 2.png"
  [3]: https://github.com/chen1218chen/JRebel6.4.2Crack
  [4]: ./images/Image%203.png "Image 3.png"
  [5]: ./images/Image4.png "Image4.png"
  [6]: ./images/Image%206.png "Image 6.png"