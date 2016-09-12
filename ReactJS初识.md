---
title: ReactJS初识
date: 2016-07-12 10:44:51
tags: ReactJS
---

[toc]
# ReactJS初识
## 虚拟DOM
Web开发中，我们经常需要将数据的时时变化反应到前台页面上，而这样就会频繁的进行DOM操作，而复杂频繁的DOM操作是会产生性能瓶颈，ReactJS为此引入了虚拟DOM的概念。在浏览器端用javascript实现了一套DOM API。基于React进行开发时的所有操作都是对虚拟的DOM进行操作，每当数据变化时，React都会重新构造DOM树，然后与上次构造的DOM树进行对比，仅对变化的部分进行实际浏览器的DOM更新。
前端任何UI的改变，都需要通过浏览器整体刷新实现。
