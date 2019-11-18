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
- 对于常见的HTML标签，React已经内置了工厂方法， 比如:
```js
var root = React.DOM.ul({className: "my-list"}, React.DOM.li(null, 'Text Content'));
```

#### 如何使用JSX
- 使用原生的HTML标签
- 使用组件
**两者约定通过大小写进行区分，小写的字符串是HTML标签， 大写开头的变量是React组件**

```js
// 使用HTML标签
var myDivElement = <div className='foo'></div>;
render(myDivElement, document.getElementById('root'));

//使用组件
var myElement = <HelloWorld name="earth" />;
```
- 使用JS表达式
```js
// input JSX
var person = <Person name={windows.isLoggedIn ? windows.name : ''} />;

// Output JS
var person = React.createElement(Person, {name: windows.isLoggedIn ? windows.name : ""});

// 子组件也可以作为表达式使用

var content = <Container> {windows.isLoggedIn ? <Nav /> : <Login />} </Container>;

var content = React.createElment(Container, null, windows.isLoggedIn ? <Nav /> : <Login />);
```

#### 注释
在 JSX 里使用注释也很简单，就是沿用 JavaScript，唯一要注意的是在一个组件的子元素位置使用注释要用 {} 包起来。
```js
var content = (
  <Nav>
      {/* child comment, put {} around */}
      <Person
        /* multi
           line
           comment */
        name={window.isLoggedIn ? window.name : ''} // end of line comment
      />
  </Nav>
);
```

### 属性扩散
有时候 一个组件有多个属性，使用者不想一个个的写下这些属性， 或者有时候你甚至不知道这些属性的名字，这个时候speed attributes的功能就很有用了
比如:
```js
var props = {};
props.foo = x;
props.bar = y;
var component = <Component {...props} />;

// 属性也可以被覆盖
var props = { foo: 'default' };
var component = <Component {...props} foo={'override'} />;
console.log(component.props.foo); // 'override'
```

## 组件
两个核心概念：
- **props**
  - 组件的属性，接收外部传入的参数
- **state**
  - 表示组件当前的状态，是一个“状态机”，根据state呈现不同的UI展示

- **划分状态数据的原则**
  - 让组件尽可能的少状态： 目的是组件容易维护
  - 什么样的数据属性可以当作状态？
    - 可计算的数据： 比如数组长度
    - 和props重复的数据

- 无状态组件
  - 你也可以用纯粹的函数来定义无状态的组件(stateless function)，这种组件没有状态，没有生命周期，只是简单的接受 props 渲染生成 DOM 结构。无状态组件非常简单，开销很低，如果可能的话尽量使用无状态组件。比如使用箭头函数定义：

```js
const HelloMessage = (props) => <div> Hello {props.name}</div>;
render(<HelloMessage name="John" />, mountNode);
```



