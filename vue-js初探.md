---
title: vue.js初探
date: 2017-02-23 17:11:10
tags: vue
---

[TOC]

## 安装使用
- [Vue中文官网][1]

vue初探，即使照着官方教程一步步来，依然是错误百出，记得最好是将node.js更新到最新版本，不然各种报错不知道怎么解决。
果不其然，再经过将环境整个更新之后在根据以下官网上的步骤一步步来就木有问题了。

    # 全局安装 vue-cli
    $ npm install --global vue-cli
> 国内的npm网速不稳定，推荐使用淘宝镜像，[点击查看详情][4]。

    # 如果安装失败，可以先clean一下，清理缓存，再重新安装
    $ npm cache clean 
    $ npm install -g cnpm --registry=https://registry.npm.taobao.org  
    $ cnpm -v //查看是否安装成功

    # 创建一个基于webpack 模板的新项目，其中webpack是模板名称
    $ vue init webpack first
    # 安装依赖，走你
    $ cd first
    $ cnpm install
    $ cnpm run dev

浏览器成功显示以下页面。
**vue-cli**：官方发布的一个vue脚手架帮助我们快速搭建vue项目，可以自动启用热加载功能。
**webpack**：是模板名称，可以到 vue.js 的 GitHub 上查看更多的模板https://github.com/vuejs-templates

![enter description here][2]

### 项目结构

![enter description here][3]
main.js:默认的主入口文件。

### 项目路径说明

![enter description here][5]

使用官方推荐的引入方法：

    <script src="https://unpkg.com/vue/dist/vue.js"></script>
## 打包上线
自己的项目文件都需要放到 src 文件夹下，项目开发完成之后，可以输入 npm run build 来进行打包工作

    npm run build

打包完成后，会生成dist文件夹，如果已经修改了文件路径，可以直接打开本地文件查看项目上线时，只需要将 dist 文件夹放到服务器就行了。

## app挂载


    new Vue({
        el: '#app',
        components: {
            app: App
        },
        router,
    }).$mount('#app')

等价于

    new Vue({
        el: '#app',    //这里绑定的是index.html中的id为app的div元素
        router,
        render: h => h(App)
    })

  [1]: https://cn.vuejs.org/
  [2]: ./images/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20170223172730.png "微信截图_20170223172730.png"
  [3]: ./images/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20170223173009.png "微信截图_20170223173009.png"
  [4]: http://npm.taobao.org/
  [5]: ./images/%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20170227101809.png "微信截图_20170227101809.png"