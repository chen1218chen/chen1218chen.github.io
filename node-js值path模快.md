---
title: node.js的path模快
date: 2017-06-06 15:01:29
tags: node.js
---
Node.js path 模块提供了一些用于处理文件路径的小工具，我们可以通过以下方式引入该模块：
创建 main.js 文件，代码如下所示：

    var path = require("path")
    // 格式化路径
    console.log('normalization : ' + path.normalize('/test/test1//2slashes/1slash/tab/..'));

    // 连接路径
    console.log('joint path : ' + path.join('/test', 'test1', '2slashes/1slash', 'tab', '..'));

    // 转换为绝对路径
    console.log('resolve : ' + path.resolve('main.js'));

    // 路径中文件的后缀名
    console.log('ext name : ' + path.extname('main.js'));
    //获取用"/"隔开的path最后的一部分
    console.log('basename: '+ path.basename('./src/views/nav1/table.js'));
代码执行结果如下：

    $ node main.js 
    normalization : /test/test1/2slashes/1slash
    joint path : /test/test1/2slashes/1slash
    resolve : /web/com/1427176256_27423/main.js
    ext name : .js
    basename : table.js
    
    