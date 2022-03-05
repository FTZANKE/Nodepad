### React和Vue的区别？(了解)
https://blog.csdn.net/qq_26190177/article/details/93741368
http://caibaojian.com/vue-vs-react.html

### 生命周期事件？
https://www.jianshu.com/p/021736302706
https://segmentfault.com/a/1190000018142773
https://www.cnblogs.com/soyxiaobi/p/9559117.html

### 课外阅读：
https://react.docschina.org/
https://blog.csdn.net/marker__/article/details/105584360

css in js:
http://www.ruanyifeng.com/blog/2017/04/css_in_js.html

1。项目命令：
yarn start启动项目
yarn build编译项目
yarn test测试项目
yarn eject暴露项目配置信息

2。yarn eject暴露react项目所有的配置信息，但不可逆。
参考：https://blog.csdn.net/gswwxyhk/article/details/100635905

3。react-scripts模块包就是脚手架使用的关键模块包，主要提供一些启动项目，编译项目，测试项目等功能，类似于vue-cli-service

4。react路由模块包分类：
react-router路由核心模块包
react-router-dom包含DOM的路由模块包（重点使用）
react-router-native原生路由模块包，一般用到react native开发，android和iOS开发时使用
react-router-config静态路由配置

5。定义局部样式?
在组件的同级目录下，创建和组件名称一样的.module.css文件，并在组件的业务逻辑中导入，使用时像对象一样去使用它。
1。创建xxx.module.css

2。xxx.js组件中导入：import css from './xxx.moudle.css';

3。使用：<div className={css.类名}

5。@loadable/component用来异步加载组件的一个模块包。







### 使用路由

1. 安装

   ```shell
   npm install react-router-dom
   ```

2. index.js 导入BrowserRouter 使用路由

   ```js
   
   // 导入路由
   import { BrowserRouter as Router } from 'react-router-dom'
   ```
   ```html
   {/* 使用 */}
   <Router>
    <App />
   </Router>
   ```

3. app.js 导入 Routes, Route, Link 使用

   ```html
   {/* 导航 */}
     <div className='menu-nav'>
       <Link to="/">首页</Link>
       <Link to="/list">列表页</Link>
     </div>
     {/* 内容 */}
     <div className='content'>
       <Routes>
         <Route path='/' element={<Home />} />
         <Route path='/list' element={<List />} />
       </Routes>
     </div>
   ```

   

### 传参 

useSearchParams

useParams

404匹配

嵌套路由







React遍历数组用map和forEach的区别:（了解）
https://www.jianshu.com/p/f73f89ef60c3