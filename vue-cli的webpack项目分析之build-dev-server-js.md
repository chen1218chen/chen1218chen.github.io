---
title: vue-cli的webpack项目分析之build/dev-server.js
date: 2017-06-09 10:00:29
tags: [vue, webpack, vue-cli]
---
[TOC]

执行"`npm run dev`" 命令的时候可以看到最先执行的是build/dev-server.js文件，所以就从`dev-server.js`文件开始分析吧，这在里mark一下，算是学习记录，也顺便跟大家分享一下。

## dev-server.js分析
> 检查node和npm的版本


    require('./check-versions')()
> 获取config目录的默认配置,并且会默认指定index.js文件


    var config = require('../config')
    if (!process.env.NODE_ENV) process.env.NODE_ENV = config.dev.env
    var path = require('path')
    var express = require('express')
    var webpack = require('webpack')
> 一个可以强制打开浏览器并跳转到指定 url 的插件


    var opn = require('opn')
> http-proxy可以实现转发所有请求代理到后端真实API地址，以实现前后端开发完全分离;
>使用 proxyTable


    var proxyMiddleware = require('http-proxy-middleware')
>根据 Node 环境来引入相应的 webpack 配置
 


    var webpackConfig = process.env.NODE_ENV === 'testing'
      ? require('./webpack.prod.conf')
      : require('./webpack.dev.conf')

    // default port where dev server listens for incoming traffic
    var port = process.env.PORT || config.dev.port
    // Define HTTP proxies to your custom API backend
    // https://github.com/chimurai/http-proxy-middleware
    var proxyTable = config.dev.proxyTable

    var app = express()
    var compiler = webpack(webpackConfig)

    var devMiddleware = require('webpack-dev-middleware')(compiler, {
      publicPath: webpackConfig.output.publicPath,
      stats: {
        colors: true,
        chunks: false
      }
    })

    var hotMiddleware = require('webpack-hot-middleware')(compiler)
    // force page reload when html-webpack-plugin template changes
    compiler.plugin('compilation', function (compilation) {
      compilation.plugin('html-webpack-plugin-after-emit', function (data, cb) {
        hotMiddleware.publish({ action: 'reload' })
        cb()
      })
    })

    // proxy api requests
    Object.keys(proxyTable).forEach(function (context) {
      var options = proxyTable[context]
      if (typeof options === 'string') {
        options = { target: options }
      }
      app.use(proxyMiddleware(context, options))
    })

    // handle fallback for HTML5 history API
    app.use(require('connect-history-api-fallback')())

    // serve webpack bundle output
    app.use(devMiddleware)

    // enable hot-reload and state-preserving
    // compilation error display
    app.use(hotMiddleware)

    // serve pure static assets
    var staticPath = path.posix.join(config.dev.assetsPublicPath, config.dev.assetsSubDirectory)
    app.use(staticPath, express.static('./static'))

    module.exports = app.listen(port, function (err) {
      if (err) {
        console.log(err)
        return
      }
      var uri = 'http://localhost:' + port
      console.log('Listening at ' + uri + '\n')
      opn(uri)
    })
    
## process.env.NODE_ENV
获取系统环境变量,在config/pro.env.js和config/dev.env.js中定义好的。
> pro.env.js


    module.exports = {
      NODE_ENV: '"production"',
      API_ROOT: '"//127.0.0.1/api"'
    }
> dev.env.js


    var merge = require('webpack-merge')
    var prodEnv = require('./prod.env')

    module.exports = merge(prodEnv, {
      NODE_ENV: '"development"',
      API_ROOT: '"//127.0.0.1/api"'
    })


从代码中看到，dev-server使用的webpack配置来自build/webpack.dev.conf.js文件（测试环境下使用的是build/webpack.prod.conf.js，这里暂时不考虑测试环境）。而build/webpack.dev.conf.js中又引用了webpack.base.conf.js，所以这里我先分析webpack.base.conf.js。
## webpack.base.conf.js
webpack.base.conf.js主要完成了下面这些事情：

- 配置webpack编译入口
- 配置webpack输出路径和命名规则
- 配置模块resolve规则
- 配置不同类型模块的处理规则
