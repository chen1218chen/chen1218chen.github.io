---
title: vue-router懒加载的实现
date: 2018-08-30 14:13:48
tags: [vue]
---
最近一直在研究用vue搭建前端平台，出现一个问题，随着前端页面的越来越多，webpack打包构建是会越来越慢，这个从每次代码修改后的热更新速度就可以看出来。
解决这个问题可以使用**路由的懒加载**，使用`Vue的异步组件`和webpack的 `Code Splitting`，很easy就实现。如：
```
const Foo = () => import('./Foo.vue')
```

## vue的异步组件

```
Vue.component('async-component', function(resolve,reject){
    resolvo({
      template: '<div>async component example</div>'
    })
})

```
也可以写成这样：
```
Vue.component('async-component', 
    () = > import('./async-component')
)
```
局部注册时：
```
new Vue ({
    ...
    components: {
        'async-component': ()=>import('./async-component')
    }
})
```
## webpack的code splitting
`webpack的code splitting`可以实现按需加载，减少首次加载时间。
作用：
1. 第三方类库单独打包
2. 按需加载，webpack支持定义分割点，通过require.ensure进行按需加载。
3. 通用模块单独打包。如一般我们会在 使用CommonChunkPlugin插件来合并公共文件，以免重复打包。

webpack遵循的原则是`All in one` ，就是从入口文件开始将所有的模块都打包到一个JS文件里面，包括引用的css、图片。打包时默认所有require都是在包文件中找，不同于requireJS可以直接require线上的URL


推荐阅读：
1. [路由懒加载][1]


  [1]: https://panjiachen.github.io/vue-element-admin-site/zh/guide/advanced/lazy-loading.html