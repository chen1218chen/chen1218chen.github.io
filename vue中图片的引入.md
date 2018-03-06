---
title: vue中图片的引入
date: 2018-03-06 13:24:49
tags: [vue, vue-cli]
---

最近项目转vue的怀抱，踩各种坑，连一个小小的图片引入功能都得重新学习探索了，不过好在越来越顺利，mark一下吧，与vue友们共勉。。。
## css中引入 `background: url("~@/assets/cloud.png");`

    <template>
      <div>
        <div class="imageBottom"></div>
        <div class="imageTop"></div>
        <div class="title"><h2>管理系统</h2></div>
      </div>
    </template>

    <script>
    export default {
      name: "Banner",
      }
    };
    </script>
    <style rel="stylesheet/scss" lang="scss" scoped>
    .imageTop {
      position: absolute;
      top: 0em;
      left: 0em;
      width: 100%;
      height: 70px;
      z-index: 9;
      background: url("~@/assets/cloud.png");
    }
    .imageBottom {
      position: absolute;
      top: 0em;
      left: 0em;
      width: 100%;
      height: 70px;
      background: url("~@/assets/background.png");
    }
    .title{
      z-index: 999;
      color: #fff;
    }
    </style>
## `<img :src='imageURL'/>`中通过参数引入

    <template>
      <div>
        <div class="title"><h2>管理系统</h2></div>
         <img :src="backgroud" alt="" width="100%" height="70" class="imageBottom">
        <img :src="cloud" alt="" width="100%" height="70" class="imageTop"> 
      </div>
    </template>

    <script>
    import backgroud from "@/assets/background.png";
    import cloud from "@/assets/cloud.png";

    export default {
      name: "Banner",
      data() {
        return {
          backgroud,
          cloud
        };
      }
    };
    </script>
    <style rel="stylesheet/scss" lang="scss" scoped>
    .imageTop {
      position: absolute;
      top: 0em;
      left: 0em;
      width: 100%;
      height: 70px;
      z-index: 9;
    .imageBottom {
      position: absolute;
      top: 0em;
      left: 0em;
      width: 100%;
      height: 70px;
    }
    .title{
      z-index: 999;
      color: #fff;
    }
    </style>
---
>初学者难免会对@产生疑惑，其实就是系统中设置的路径别名而已，在webpack.base.conf.js中设置

    resolve: {
        extensions: ['.js', '.vue', '.json'],
        alias: {
          'vue$': 'vue/dist/vue.esm.js',
          '@': resolve('src'),
        }
    },