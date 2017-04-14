---
title: vue-router之钩子函数
date: 2017-04-14 10:21:02
tags: vue
---
[TOC]

## 钩子函数
路由的切换过程，本质上是执行一系列路由钩子函数，钩子函数总体上分为两大类：

- 全局的钩子函数
- 组件的钩子函数

全局的钩子函数定义在全局的路由对象中，组件的钩子函数则定义在组件的route选项中。

### 全局钩子函数
全局钩子函数有2个：
- **beforeEach**：在路由切换开始时调用
项目中用到的路由钩子函数来进行登陆判断


    router.beforeEach((to, from, next) => {
      //如果跳转到登陆界面
      if (to.path == '/login') {
        sessionStorage.removeItem('user');
      }
      let user = JSON.parse(sessionStorage.getItem('user'));
      if (!user && to.path != '/login') {
        next({ path: '/login' })
      } else {
        next()
      }
    })
    
- **afterEach**：在每次路由切换成功进入激活阶段时被调用
### 组件的钩子函数
组件的钩子函数一共6个：

- **data**：可以设置组件的data
- **activate**：激活组件
- **deactivate**：禁用组件
- **canActivate**：组件是否可以被激活
- **canDeactivate**：组件是否可以被禁用
- **canReuse**：组件是否可以被重用
### 切换对象
每个切换钩子函数都会接受一个 transition 对象作为参数。这个切换对象包含以下函数和方法：

- **transition.to** 
表示将要切换到的路径的路由对象。
- **transition.from** 
代表当前路径的路由对象。
- **transition.next()** 
调用此函数处理切换过程的下一步。
- **transition.abort([reason])** 
调用此函数来终止或者拒绝此次切换。
- **transition.redirect(path)** 
取消当前切换并重定向到另一个路由。

### 钩子函数的执行顺序
1. 可重用阶段 
检查当前的视图结构中是否存在可以重用的组件 ,`canReuse`
2. 验证阶段
检查当前的组件是否能够停用以及新组件是否可以被激活, `canDeactivate` 和`canActivate` 
3. 激活阶段
一旦所有的验证钩子函数都被调用而且没有终止切换，切换就可以认定是合法的。路由器则开始禁用当前组件并启用新组件。
此阶段对应钩子函数的调用顺序和验证阶段相同，其目的是在组件切换真正执行之前提供一个进行清理和准备的机会。界面的更新会等到所有受影响组件的`deactivate` 和 `activate `钩子函数执行之后才进行。
4. 执行`data`


