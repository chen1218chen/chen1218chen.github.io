---
title: hexo的使用 
tags: [hexo]
grammar_cjkRuby: true
---
[TOC]


## 利用npm安装
    

    npm install -g hexo;
    hexo init <folder>;
    npm stall;//安装依赖包
    hexo new "postName" #新建文章
    hexo new page "pageName" #新建页面
    hexo generate;
    hexo server;
    
   至此本地hexo博客已经搭建起来了，Http://localhost:4000查看

    #清除缓存
    hexo clean
    
## github
编辑_config.yml
   

     deploy:
      type: github
      repository: https://github.com/zippera/zippera.github.io.git
      branch: master

据说最新版本的hexo 中，这里的 type 要写成 git，而不是 github。
执行下列指令即可完成部署。

    hexo generate
    hexo deploy //同步到github
    
    //指的是生成后马上部署站点
    hexo g -d or 
    hexo d -g 

**ERROR Deployer not found : github**
解决方法如下：
deploy的type改成git，然后运行下

    npm install hexo-deployer-git --save
    hexo g
    hexo d
    
**ERROR：failed to execute prompt script (exit code 1)**
解决方法如下：

    deploy:
      type: git
      repository: ssh://git@github.com/chen1218chen/chen1218chen.github.io.git
      branch: master
      
git连接测试

     ssh -T git@github.com
## 开启RSS功能 
安装RSS,，并且编辑hexo/_config.yml，添加如下代码：

    npm install hexo-generator-feed --save
    rss: /atom.xml #rss地址  默认即可
    
## 命令简化
hexo现在支持更加简单的命令格式了，比如：

    hexo g == hexo generate
    hexo d == hexo deploy
    hexo s == hexo server
    hexo n == hexo new
    
## 简单
    hexo n #写文章
    hexo g #生成
    hexo d #部署 # 可与hexo g合并为 hexo d -g