组件传值
参考：
https://www.jianshu.com/p/e95ee2d71bad
https://blog.csdn.net/q3254421/article/details/82927860

动态过渡路由
https://router.vuejs.org/zh/guide/advanced/transitions.html#基于路由的动态过渡
https://www.cnblogs.com/yangshifu/p/9086830.html
https://blog.csdn.net/ZYS10000/article/details/108435086
https://blog.csdn.net/qq_33236453/article/details/79110485


## Vuex
**vuex概念？使用场景？ **
概念：vuex是vue提供的数据共享方案（即：状态管理方案），旨在集中式（唯一的数据源）管理状态数据，让它在各个组件中共享。
使用场景：一般管理需要在多数组件中共享的数据。应用在大型项目中。
比如：登录状态，不经常变化的数据字典（如：收货地址的省市区数据）

**vuex状态管理中的几大对象及特点？**

### 1. state: 

**状态数据，单一状态树，整个应用程序只有一个唯一数据源。**

访问state：$store.state  mapState

### 2. getters: 

**用来对state做进一步的查询、筛选、过滤操作。**

访问getters：$store.getters   mapGetters

### 3. mutations: 
**是更改state的唯一方式，且是同步更改状态数据，业务逻辑复杂时会阻塞。**

访问mutations：$store.commit(）  mapMutations

**commit(）**第一个参数是mutations里的事件名;第二个参数是可传输数据【没有第三个参数】

每个 mutation 都有一个字符串的 **事件类型 (type)** 和 一个 **回调函数 (handler)**

### 4. actions: 

**不直接更改state，但可以调用mutations让其帮actions去更改state。且可以同步和异步更改数据。在actions中可以调用多个mutations，其实是对mutations做进一步的封装操作，让更改state的行为可以是同步的，也可以是异步的。**

访问actions：$store.dispatch()  mapActions

### 5. modules: 
**开发大型项目时，需要管理各个功能模块的数据状态，分模块化开发，让代码易维护。**





**文档参考：**

vuex：
https://vuex.vuejs.org/zh/

vuex-persist：
https://www.npmjs.com/package/vuex-persist

vue-devtools：
https://github.com/vuejs/vue-devtools

chrome浏览器上的下载地址：
https://chrome.google.com/webstore/detail/vuejs-devtools/nhdogjmejiglipccpnnnanhbledajbpd

映射函数：mapState,mapGetters,mapMutations,mapActions

mapState,mapGetters映射到计算属性computed上。
mapMutations,mapActions映射到方法methods上。




