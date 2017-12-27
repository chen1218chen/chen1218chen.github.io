---
title: babel
date: 2017-09-11 16:53:56
tags:
---
babel: 转码器, 可以将ES6转为ES5
## .babelrc
babel配置文件

    {
        #设定转码规则
        "presets": [
          "es2015",
          "react",
          "stage-2"
        ],
        "plugins": []
    }
官方提供以下的规则集，根据需要安装

    # ES2015转码规则
    $ npm install --save-dev babel-preset-es2015

    # react转码规则
    $ npm install --save-dev babel-preset-react

    # ES7不同阶段语法提案的转码规则（共有4个阶段），选装一个
    $ npm install --save-dev babel-preset-stage-0
    $ npm install --save-dev babel-preset-stage-1
    $ npm install --save-dev babel-preset-stage-2
    $ npm install --save-dev babel-preset-stage-3
## babel-loader

官方的说法是：This package allows transpiling JavaScript files using Babel and webpack.即用babel+webpack来转译Javascript。

## babel-register
babel-register模块改写require命令，为它加上一个钩子。此后，每当使用require加载.js、.jsx、.es和.es6后缀名的文件，就会先用Babel进行转码。

## babel-core
## babel-polyfill
Babel默认只转换新的JavaScript句法（syntax），而不转换新的API，比如Iterator、Generator、Set、Maps、Proxy、Reflect、Symbol、Promise等全局对象，以及一些定义在全局对象上的方法（比如Object.assign）都不会转码。
举例来说，ES6在Array对象上新增了Array.from方法。Babel就不会转码这个方法。如果想让这个方法运行，必须使用babel-polyfill，为当前环境提供一个垫片。

参考文献：
[babel入门教程][1]


  [1]: http://www.ruanyifeng.com/blog/2016/01/babel.html