---
title: css3 filter滤镜属性
date: 2016-05-13 14:26:59
tags: css3
---
## 网页变灰原理
使用css3的灰度过滤功能

     .icon_f{
           -webkit-filter: grayscale(100%);
           -moz-filter: grayscale(100%);
           -ms-filter: grayscale(100%);
           -o-filter: grayscale(100%);    
           filter: grayscale(100%);
           filter: gray;
       }
## 模糊图片      

    img{
        -webkit-filter: blur(5px); /* Chrome, Safari, Opera */
        filter: blur(5px);
    }
    
## 图片变亮

    img {
        -webkit-filter: brightness(200%); /* Chrome, Safari, Opera */
        filter: brightness(200%);
    }
## 对比度调整

    img {
        -webkit-filter: contrast(200%); /* Chrome, Safari, Opera */
        filter: contrast(200%);
    }
## 添加阴影效果

    img {
        -webkit-filter: drop-shadow(8px 8px 10px red); /* Chrome, Safari, Opera */
        filter: drop-shadow(8px 8px 10px red);
    }
    
除此之外

    .contrast {
        -webkit-filter: contrast(180%);
        filter: contrast(180%);
    }
    //给图像应用色相旋转
    .huerotate {
        -webkit-filter: hue-rotate(180deg);
        filter: hue-rotate(180deg);
    }
    //反转输入图像。
    .invert {
        -webkit-filter: invert(100%);
        filter: invert(100%);
    }
    //转化图像的透明程度。
    .opacity {
        -webkit-filter: opacity(50%);
        filter: opacity(50%);
    }
    //转换图像饱和度
    .saturate {
        -webkit-filter: saturate(7);
        filter: saturate(7);
    }
    //将图像转换为深褐色。
    .sepia {
        -webkit-filter: sepia(100%);
        filter: sepia(100%);
    }
 