---
title: 01学习React
categories:
  - 前端
tags:
  - React
  - JSX
type: home
comments: true
date: 2019-10-25 23:14:32
---

## 学习React
---
**随便记录，并无相关结构化信息文章**

### 一些重要的特性和概念
- React是FaceBook的开源项目用于构建facebook和Instagram， 是一个UI库；目前已经发展为一种生态圈
- 核心思想：**封装组件**
  - 各个组件维护自己的state 和 UI，当state变化时，自动调用render方法重新渲染整个组件；
    - 相比jquery等的优势，不用操作DOM对象，查找某一个DOM元素，然后对其进行更新UI
    - **组件的state只能通过setState()方法进行更新**
    - **React**是一个状态机，通过状态来更新组件
    - 更新组件，实际上是通过VirtualDOM来完成的，实际并没有大面更新所有的DOM对象
- 核心概念：
  - 组件component
  - JSX
  - Virtual DOM
  - Data Flow
- 实际例子
```js
import React, { Component } from 'react';
import { render } from 'react-dom'

// 组件名称必须大写
class HelloWorld exntends Component {
  render() {
    return <div> Hello world, {this.props.name}</div>
  }
}

export default HelloWorld;

// 加载到页面
render(<HelloWorld name="Earth" />, document.getElementById("root));
```  

### WebPack
**WebPack**是一个前端的资源加载和打包工具，只需要相对简单的配置就可以提供前端工程化需要的各种功能，并且如果雨需要他还可以被整合到比如Grunt、Gulp的工作流中。
`npm install -g webpack`
[webpack中文网站](https://www.webpackjs.com)

### JSX
**JSX** : JavaScript Xml, 编译JSX通常使用[**Babel**](https://www.babeljs.cn/)
**注意**： JSX中对class和for，需要用className,HtmlFor来代替，避免出现与JS的关键字进行冲突
- Babel编译JSX后输出的是JS代码，比如以下HSX编写的一个链接：
```js
<a href="http://facebook.github.io/react/">Hello!</a>
```
用JS代码来写就是这个样子：
```js
React.createElement('a', {href: 'http://facebook.github.io/react/'}, 'Hello!');
```
你可以通过 React.createElement 来构造组件的 DOM 树。第一个参数是标签名，第二个参数是属性对象，第三个参数是子元素。
- 一个包含子元素的例子：
  ```js
  var child = React.createElement('li', null, 'Text Content');
  var root = React.createElement('ul', {className: 'my-list'}, child);
  React.render(root, document.body);
  ```

