## Vue双向数据绑定原理

#### 1. [vue](https://so.csdn.net/so/search?from=pc_blog_highlight&q=vue)双向数据绑定是通过 数据劫持，并结合 发布-订阅模式的方法来实现的，也就是说数据和视图同步，数据发生变化，视图跟着变化，视图变化，数据也随之发生改变

#### 2. 核心：关于vue实现双向数据绑定，其核心是Object.defineProperty()方法