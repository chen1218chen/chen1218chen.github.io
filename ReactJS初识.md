---
title: ReactJS初识
date: 2016-07-12 10:44:51
tags: ReactJS
---

[toc]
# ReactJS初识
## 组件周期
组件的生命周期可以分为三个阶段：Mounting, Updating, Unmonuting。
**Mounting**： 组件被插入DOM中
**Updating**：DOM数据被更新，组件需要重新渲染
**Unmounting**: 从DOM中杀出组件
React在每个阶段开始前提供了will方法用于在这一阶段开始前的操作。did方法用于执行每个阶段结束之后的操作。
>Mounting阶段

首先在Mounting阶段，Component通过React.createClass被创建出来，然后调用 getInitialState 方法初始化 this.state。在 Component 执行 render 方法之前，通过调用 componentWillMount（方法修改 state 状态），然后执行 render。Reder的过程即是组件生成HTML结构的过程。在render之后，Component 会调用 componentDidMount 方法。在这个方法执行之后，开发人员才能通过 this.getDOMNode()获取到组件的 DOM 节点。
- getInitialState()
- componentWillMount(): mounting occurs之前立即调用
- componentDidMount(): mounting occurs之后立即调用，需要初始化DOM节点的操作都在这里。
>Updating阶段

当 Component 在 mount 结束之后，它当中有任何数据修改导致的更新都会在 Updating 阶段执行。Component 的 componentWillReceiveProps 方法会监听组件中 props。监听到 props 发生修改，它会比对新的数据与之前的数据之间是否存在差别进而修改 state 的值。当比对的结果为数据变化需要对 Component 对应的 DOM 节点做出修改的时候，shouldCoponentUpdate 方法它会返回 true 用于触发 componentWillUpdate 和 componentDidUpdate 方法。在默认的情况下 shouldComponentUpdate 返回为 true。有些特殊的情况是当 component 中的 props 发生修改，但是其本身数据并没有改变，或者是开发人员手工设置 shouldComponentUpdate 为 false 时，React 就不会更新这个 component 对应的 DOM 节点了。与 componentWillMount 和 componentDidMount 相类似，componentWillUpdate 和 componentDidUpdate 也分别在组件更新的 render 过程前后执行。
>Unmounting阶段

当开发人员需要将 component 从 DOM 中移除时，就会触发 UnMounting 阶段。在这个阶段中，React 只提供了一个 componentWillUnmount 方法在卸载和销毁这个 component 之前触发，用于删除 component 中的 DOM 元素等。

## 虚拟DOM
Web开发中，我们经常需要将数据的时时变化反应到前台页面上，而这样就会频繁的进行DOM操作，而复杂频繁的DOM操作是会产生性能瓶颈，ReactJS为此引入了虚拟DOM的概念。在浏览器端用javascript实现了一套DOM API。基于React进行开发时的所有操作都是对虚拟的DOM进行操作，每当数据变化时，React都会重新构造DOM树，然后与上次构造的DOM树进行对比，仅对变化的部分进行实际浏览器的DOM更新。
前端任何UI的改变，都需要通过浏览器整体刷新实现。

## ref回调属性

    render: function() {
    return (
      <TextInput
        ref={function(input) {
          if (input != null) {
            input.focus();
          }
        }} />
    );
    },
或者使用ES6的arrow function

    render: function() {
        return <TextInput ref={(c) => this._input = c} />;
      },
      componentDidMount: function() {
        this._input.focus();
      },
this callback will be executed immediately after the component is mounted
参考文献：
1. [React 介绍及实践教程][1]
2. [官网教程][2]


  [1]: http://www.ibm.com/developerworks/cn/web/1509_dongyue_react/index.html
  [2]: https://facebook.github.io/react/docs/tutorial.html