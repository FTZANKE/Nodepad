学习文档：
菜鸟：
https://www.runoob.com/react/react-tutorial.html

react官方：
https://react.docschina.org/

https://zh-hans.reactjs.org/

#### 什么是react？ 特点？
React 是一个用于构建用户界面的 JAVASCRIPT 库（框架）。
React 主要用于构建UI，很多人认为 React 是 MVC 中的 V（视图）。
React 起源于 Facebook 的内部项目，用来架设 Instagram 的网站，并于 2013 年 5 月开源。
React 拥有较高的性能，代码逻辑非常简单，越来越多的人已开始关注和使用它。

#### react 特点

> 重点强调**组件**概念

1. **声明式设计** −React采用声明范式，可以轻松描述应用。
2. **高效** −React通过对DOM的模拟，最大限度地减少与DOM的交互。
3. **灵活** −React可以与已知的库或框架很好地配合。
4. **JSX** − JSX 是 JavaScript 语法的扩展。React 开发不一定使用 JSX ，但我们建议使用它。
5. **组件** − 通过 React 构建组件，使得代码更加容易得到复用，能够很好的应用在大项目的开发中。
6. **单向响应的数据流** − React实现了单向响应的数据流，从而减少了重复代码，这也是它为什么比传统数据绑定更简单。


#### React中几个核心的概念

##### 虚拟DOM（Virtual Document Object Model）

- **DOM的本质是什么**：浏览器中的概念，用JS对象来表示 页面上的元素，并提供了操作 DOM 对象的API；
- **什么是React中的虚拟DOM**：是框架中的概念，是程序员 用JS对象来模拟 页面上的 DOM 和 DOM嵌套；
- **为什么要实现虚拟DOM（虚拟DOM的目的）：**为了实现页面中， DOM 元素的高效更新
  - **DOM和虚拟DOM的区别**：
  - **DOM：**浏览器中，提供的概念；用JS对象，表示页面上的元素，并提供了操作元素的API；
  - **虚拟DOM：**是框架中的概念；而是开发框架的程序员，手动用JS对象来模拟DOM元素和嵌套关系；
    - 本质： 用JS对象，来模拟DOM元素和嵌套关系；
    - 目的：就是为了实现页面元素的高效更新；

##### Diff算法：

- **tree diff:**新旧两棵DOM树，**逐层对比**的过程，就是 Tree Diff； 当整颗DOM逐层对比完毕，则所有需要被按需更新的元素，必然能够找到；

- **component diff：**在进行Tree Diff的时候，每一层中，组件级别的对比，叫做 Component Diff；

  - 如果对比前后，组件的类型相同，则**暂时**认为此组件不需要被更新；
  - 如果对比前后，组件类型不同，则需要移除旧组件，创建新组件，并追加到页面上；

- **element diff:**在进行组件对比的时候，如果两个组件类型相同，则需要进行 元素级别的对比，这叫做 Element Diff；

  

#### react如何应用？
a. 在浏览器端使用，下载相应的核心脚本库，引入，操作相应的react框架API就可以使用了。

```html
<!-- 
1。引入核心脚本库
react.development.js是react框架核心脚本库
react-dom.development.js是react框架提供操作虚拟DOM的重要脚本库
babel.min.js是用来转码的，react框架中使用jsx语法渲染视图，jsx语法浏览器默认不支持，需要使用babel转码。
-->
<script src="https://cdn.staticfile.org/react/16.4.0/umd/react.development.js"></script>
<script src="https://cdn.staticfile.org/react-dom/16.4.0/umd/react-dom.development.js"></script>
<!-- 生产环境中不建议使用 -->
<script src="https://cdn.staticfile.org/babel-standalone/6.26.0/babel.min.js"></script>
```



b. 使用react脚手架create-react-app创建单页面应用程序。（今天先演示一下，后面再详谈）
全局安装：npm i create-react-app -g 
查看版本：create-react-app --version
创建项目：create-react-app 项目名称

安装node自动安装npm,npm中提供的npx

yarn start =>npm start
yarn build =>npm run build

#### 重点撑握内容？
1. 元素渲染
   - render渲染虚拟DOM到app容器。虚拟DOM，即visual DOM => VNode
2. jsx语法
   - jsx语法，是react框架提供的，是对js的扩展，浏览器默认不支持，需要使用babel转码。
      - jsx看似像一段html片段，其实是js。jsx主要用来渲染元素，灵活，方便，react推荐使用。
      - 注释
      - 插值语法
      - props
3. 组件化
   - 函数组件=>纯函数组件（失血组件）：只有静态视图，没有动态数据。
   - 类组件  es6中class
     - React.Component是react框架提供的定义组件关键对象，此对象包含大量的组件API及组件生命周期钩子事件。
     - 数据绑定
     - 双向数据绑定
4. 条件渲染
5. 列表渲染
6. 生命周期
7. ajax请求



> 绑定事件的两种方式？
> 使用箭头函数纠正this
> 在构造函数中通过bind纠正this

#### 生命周期
三个状态:
Mounting：已插入真实 DOM(已挂载，beforeMount和mounted)
Updating：正在被重新渲染（数据更新，视图重新渲染 beforeUpdate和updated）
Unmounting：已移出真实 DOM（销毁，beforeDestroy和destroyed）



**componentWillMount**在渲染前调用,在客户端也在服务端。
**componentDidMount :**在第一次渲染后调用，只在客户端。之后组件已经生成了对应的DOM结构，可以通过this.getDOMNode()来进行访问。 如果你想和其他JavaScript框架一起使用，可以在这个方法中调用setTimeout, setInterval或者发送AJAX请求等操作(防止异步操作阻塞UI)。
**componentWillReceiveProps**在组件接收到一个新的 prop (更新后)时被调用。这个方法在初始化render时不会被调用。
**shouldComponentUpdate**返回一个布尔值。在组件接收到新的props或者state时被调用。在初始化时或者使用forceUpdate时不被调用。
可以在你确认不需要更新组件时使用。
**componentWillUpdate**在组件接收到新的props或者state但还没有render时被调用。在初始化时不会被调用。
**componentDidUpdate** 在组件完成更新后立即调用。在初始化时不会被调用。
**componentWillUnmount**在组件从 DOM 中移除之前立刻被调用。

componentWillMount 模块渲染前
componentDidMount 模块渲染后
componentWillReceiveProps 模块将接受新的数据
shouldComponentUpdate 判断模块需不需要重新渲染
componentWillUpdate 在组件接收到新的数据但还没有渲染时被调用。在初始化时不会被调用
componentDidUpdate 组件完成更新后
componentWillUnmount 组件从 DOM 中移除之前
