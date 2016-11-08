---
title: Sublime Angular snippets安装
date: 2016-11-08 15:12:44
tags: [Sublime, angular]
---

这是john papa写的一个sublime插件，安装后可快速生成angular的模板，指令

    ngcontroller // creates an Angular controller
    ngdirective  // creates an Angular directive
    ngfactory    // creates an Angular factory
    ngmodule     // creates an Angular module
    ngservice    // creates an Angular service
    ngfilter     // creates an Angular filter

> js文件中输入ngfactory，按下Tab键之后...


    (function() {
        'use strict';

        angular
            .module('module')
            .factory('factory', factory);

        factory.$inject = ['dependencies'];

        /* @ngInject */
        function factory(dependencies) {
            var service = {
                func: func
            };
            return service;

            function func() {
            }
        }
    })();
地址：[johnpapa/angular-styleguide][1]
下载下来之后，copy `angular-styleguide/a1/assets/sublime-angular-snippets`，放在sublime下的packages文件夹中，重启sublime即可。
出现一个问题，放入后没有反应，后来才发现未知不对，默认的sublime的插件安装地址是`C:\Users\Administrator\AppData\Roaming\Sublime Text 3\Packages`，所以将`sublime-angular-snippets`文件夹放入其中即可。


  [1]: https://github.com/johnpapa/angular-styleguide