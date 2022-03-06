## 1.什么是vue的生命周期？

> Vue实例从创建到销毁的过程，就是生命周期。也就是从开始创建、初始化数据、编译模板、挂载DOM->渲染、更新->渲染、卸载等一系列过程，我们称这是Vue的生命周期。

## 2.vue生命周期的作用是什么？

> 它的生命周期中有多个事件钩子，让我们在控制整个vue实例的过程时更容易形成好的逻辑。

## 3.Vue生命周期总共有几个阶段？

> 它可以总共分为8个阶段：创建前/后,载入前/后，更新前/后，销毁前/销毁后

## 4.第一次页面加载会触发那几个钩子？

> 第一次页面加载时会触发beforeCreate,created,beforeMount,mounted

## 5.DOM渲染在哪个周期中就已经完成？

> DOM渲染在mounted中就已经完成了

## 6.生命周期钩子的一些使用方法：

```js
1.beforecreate:可以在加个loading事件，在加载实例是触发
2.created:初始化完成时的事件写在这里，如在这结束loading事件，异步请求也适宜在这里调用
3.mounted:挂载元素，获取到dom节点
4.updated:如果对数据统一处理，在这里写上相应函数
5.beforeDestroy:可以一个确认停止事件的确认框
6.nextTick:更新数据后立即操作dom
```

## **7.v-show与v-if的区别**

> v-show是css切换，v-if是完整的销毁和重新创建
> 使用频繁切换时用v-show,运行时较少改变时用v-if
> V-if=’false’v-if是条件渲染，当false的时候不会渲染
> 使用v-if的时候，如果值为false，那么页面将不会有这个html标签生成
> v-show则是不管值是为true还是false，html元素都会存在，只是css中的display显示或隐藏
> v-show 仅仅控制元素的显示方式，将 display 属性在 block 和 none 来回切换；而v-if会控制这个 DOM 节点的存在与否。当我们需要经常切换某个元素的显示/隐藏时，使用v-show会更加节省性能上的开销；当只需要一次显示或隐藏时，使用v-if更加合理。

## **8.开发中常用的指令有哪些?**

> v-model:一般用在表达输入，很轻松的实现表单控件和数据的双向绑定
> v-html：更新元素的innerHTML
> v-show与v-if：条件渲染，注意二者区别
> v-on:click:可以简写为@click,@绑定一个事件。如果事件触发了，就可以指定事件的处理函数
> v-for：基于源数据多次渲染元素或模板
> v-bind:当表达式的值改变时，将其产生的连带影响，响应式地作用于DOM语法
> v-bind:title=”msg”简写：title="msg"

## **9.绑定class的数组用法**

```js
1.对象方法v-bind:class="{'orange':isRipe, 'green':isNotRipe}”
2.数组方法v-bind:class="[class1,class2]"
3.行内v-bind:style="{color:color,fontSize:fontSize+'px'}”
```

## **10.路由跳转方式**

> 1.router-link标签会渲染为标签，咋填template中的跳转都是这种；
> 2.另一种是编辑是导航，也就是通过js跳转比如router.push('/home')

## **11.MVVM**

> M-model，model代表数据模型，也可以在model中定义数据修改和操作的业务逻辑
>
> V-view,view代表UI组件，它负责将数据模型转化为UI展现出来
>
> VM-viewmodel,viewmodel监听模型数据的改变和控制视图行为、处理用户交互，简单理解就是一个同步view和model的对象，连接model和view

## 12.**computed和watch有什么区别**

**computed**

> computed是计算属性，也就是计算值，它更多用于计算值的场景
> computed具有缓存性，computed的值在getter执行后是会缓存的，只有在它依赖的属性值改变之后，下一次获取computed的值时重新调用对应的getter来计算
> computed适用于计算比较消耗性能的计算场景

**watch**

> watch更多的是[观察]的作用，类似于某些数据的监听回调，用于观察props $emit或者本组件的值，当数据变化时来执行回调进行后续操作
> 无缓存性，页面重新渲染时值不变化也会执行

**小结**

> 当我们要进行数值计算，而且依赖于其他数据，那么把这个数据设计为computed
> 如果你需要在某个数据变化时做一些事情，使用watch来观察这个数据变化。

## 13.**vue组件的scoped属性的作用**

> 在style标签上添加scoped属性，以表示它的样式作用于当下的模块，很好的实现了样式私有化的目的；
> 但是也得慎用：样式不易（可）修改，而很多时候，我们是需要对公共组件的样式做微调的；

**解决办法：**

> ①：使用混合型的css样式：（混合使用全局跟本地的样式） <style> /* 全局样式 */ </style><style scoped> /* 本地样式 */ </style>
> ②：[深度作用选择器](https://www.zhihu.com/search?q=深度作用选择器&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"article"%2C"sourceId"%3A97950650})（>>>）如果你希望 scoped 样式中的一个选择器能够作用得“更深”，例如影响子组件，你可以使用 >>> 操作符：<style scoped> .a >>> .b { /* ... */ } </style>

## 14.**vue是渐进式的框架的理解：(**主张最少,没有多做职责之外的事**)**

> Vue的核心的功能，是一个[视图模板引擎](https://www.zhihu.com/search?q=视图模板引擎&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"article"%2C"sourceId"%3A97950650})，但这不是说Vue就不能成为一个框架。如下图所示，这里包含了Vue的所有部件，在声明式渲染（视图模板引擎）的基础上，我们可以通过添加组件系统、客户端路由、大规模状态管理来构建一个完整的框架。更重要的是，这些功能相互独立，你可以在核心功能的基础上任意选用其他的部件，不一定要全部整合在一起。可以看到，所说的“渐进式”，其实就是Vue的使用方式，同时也体现了Vue的设计的理念
> 在我看来，渐进式代表的含义是：主张最少。视图模板引擎
> 每个框架都不可避免会有自己的一些特点，从而会对使用者有一定的要求，这些要求就是主张，主张有强有弱，它的强势程度会影响在业务开发中的使用方式。
> 比如说，Angular，它两个版本都是强主张的，如果你用它，必须接受以下东西：
> 必须使用它的模块机制- 必须使用它的依赖注入- 必须使用它的特殊形式定义组件（这一点每个视图框架都有，难以避免）
> 所以Angular是带有比较强的排它性的，如果你的应用不是从头开始，而是要不断考虑是否跟其他东西集成，这些主张会带来一些困扰。
> Vue可能有些方面是不如React，不如Angular，但它是渐进的，没有强主张，你可以在原有大系统的上面，把一两个组件改用它实现，当jQuery用；也可以整个用它全家桶开发，当Angular用；还可以用它的视图，搭配你自己设计的整个下层用。也可以函数式，都可以，它只是个轻量视图而已，只做了自己该做的事，没有做不该做的事，仅此而已。
> **渐进式的含义，我的理解是：没有多做职责之外的事。**

## **15.vue.js的两个核心是什么(数据驱动、组件系统。)**

> 数据驱动:Object.defineProperty和存储器属性: getter和setter（所以只兼容IE9及以上版本），可称为基于依赖收集的观测机制,核心是VM，即ViewModel，保证数据和视图的一致性。
> 组件系统:[点此查看](https://link.zhihu.com/?target=https%3A//blog.csdn.net/tangxiujiang/article/details/79620542%23commentBox)

## 16.vue常用修饰符

> **修饰符分为：一般修饰符，事件修身符，按键、系统**
> ①一般修饰符：
> .lazy：v-model 在每次 input 事件触发后将输入框的值与数据进行同步 。你可以添加 lazy 修饰符，从而转变为使用 change 事件进行同步

```js
<input v-model.lazy="msg" >  
```

**.number**

```js
<input v-model.number="age" type="number">
```

**.trim**

```js
1.如果要自动过滤用户输入的首尾空白字符 <input v-model.trim='trim'>
```

> **② 事件修饰符**

```js
<a v-on:click.stop="doThis"></a><!-- 阻止单击事件继续传播 -->

<form v-on:submit.prevent="onSubmit"></form> <!-- 提交事件不再重载页面 -->

<a v-on:click.stop.prevent="doThat"></a> <!-- 修饰符可以串联 -->

<form v-on:submit.prevent></form>   <!-- 只有修饰符 -->

<div v-on:click.capture="doThis">...</div>   <!-- 添加事件监听器时使用事件捕获模式 --> <!-- 即元素自身触发的事件先在此处处理，然后才交由内部元素进行处理 -->

<div v-on:click.self="doThat">...</div>  <!-- 只当在 event.target 是当前元素自身时触发处理函数 --> <!-- 即事件不是从内部元素触发的 -->

<a v-on:click.once="doThis"></a> <!-- 点击事件将只会触发一次 -->
```

> **③[按键修饰符](https://www.zhihu.com/search?q=按键修饰符&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"article"%2C"sourceId"%3A97950650})**
> 全部的按键别名:

```js
.enter
.tab
.delete (捕获“删除”和“退格”键)
.esc
.space
.up
.down
.left
.right
.ctrl
.alt
.shift
.meta
```



```js
<input v-on:keyup.enter="submit"> 或者 <input @keyup.enter="submit">
```

> **④系统修饰键 （可以用如下修饰符来实现仅在按下相应按键时才触发鼠标或键盘事件的监听器。）**

```js
.ctrl
.alt
.shift
.meta
```



```js
<input @keyup.alt.67="clear"> 或者 <div @click.ctrl="doSomething">Do something</div><!-- Ctrl + Click -->
```

## 17.**v-on可以监听多个方法吗？（可以的）**

> 一个元素绑定多个事件的两种写法，一个事件绑定多个函数的两种写法，修饰符的使用。

```js
<a style="cursor:default" v-on='{click:DoSomething,mouseleave:MouseLeave}'>doSomething</a>
```

> 在method方法里面分别写两个时事件；

```js
<button @click="a(),b()">点我ab</button>
```

## 18.**vue事件中如何使用event对象**

```js
<button @click="Event($event)">事件对象</button>
```

## 19.**比如你想让一个dom元素显示**，然后下一步去获取这个元素的offsetWidth，最后你获取到的会是0。

> 因为你改变数据把show变成true,元素并不会立即显示，理所当然也不会获取到[动态宽度](https://www.zhihu.com/search?q=动态宽度&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"article"%2C"sourceId"%3A97950650})。
> 正确的做法是先把元素show出来，在$nextTick去执行获取宽度的操作，不知道这样说会不会好理解一点。

```js
openSubmenu() {
this.show = true //获取不到宽度
this.$nextTick(() => //这里才可以 let w = this.$refs.submenu.offsetWidth;
})
}
```

## 20.**Vue 组件中 data 为什么必须是函数**

> vue组件中data值不能为对象，因为对象是引用类型，组件可能会被多个实例同时引用。
> 如果data值为对象，将导致多个实例共享一个对象，其中一个组件改变data属性值，其它实例也会受到影响。

## 21.**vue中子组件调用父组件的方法**

> 第一种方法是直接在子组件中通过this.$parent.event来调用父组件的方法
> 第二种方法是在子组件里用$emit向父组件触发一个事件，父组件监听这个事件就行了。
> 第三种都可以实现子组件调用父组件的方法，

```js
<template>
  <div>
    <button @click="childMethod()">点击</button>
  </div>
</template>
<script>
  export default {
    props: {
      fatherMethod: {
        type: Function,
        default: null
      }
    },
    methods: {
      childMethod() {
        if (this.fatherMethod) {
          this.fatherMethod();
        }
      }
    }
  };
</script>
```

## 22.**vue中 keep-alive 组件的作用**

> keep-alive 是 Vue 内置的一个组件，可以使被包含的组件保留状态，或避免重新渲染。

```js
<keep-alive>
  <component>
    <!-- 该组件将被缓存！ -->
  </component>
</keep-alive>
如果只想 router-view 里面某个组件被缓存


export default [
  {
    path: '/',
    name: 'home',
    component: Home,
    meta: {
      keepAlive: true // 需要被缓存
    }
  }, {
    path: '/:id',
    name: 'edit',
    component: Edit,
    meta: {
      keepAlive: false // 不需要被缓存
    }
  }
]
<keep-alive>
    <router-view v-if="$route.meta.keepAlive">
        <!-- 这里是会被缓存的视图组件，比如 Home！ -->
    </router-view>
</keep-alive>
 
<router-view v-if="!$route.meta.keepAlive">
    <!-- 这里是不被缓存的视图组件，比如 Edit！ -->
</router-view>
```

## 23.**vue中如何编写可复用的组件？**

[去这里看一下blog.csdn.net/qq_38563845/article/details/77524934](https://link.zhihu.com/?target=https%3A//blog.csdn.net/qq_38563845/article/details/77524934)

> ①创建组件页面eg Toast.vue；
> ②用Vue.extend()扩展一个组件构造器,再通过实例化组件构造器,就可创造出可复用的组件
> ③将[toast组件](https://www.zhihu.com/search?q=toast组件&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"article"%2C"sourceId"%3A97950650})挂载到新创建的div上；
> ④把toast组件的dom添加到body里；
> ⑤修改优化达到动态控制页面显示文字跟显示时间；

```js
import Vue from 'vue'; 
import Toast from '@/components/Toast';     //引入组件
let ToastConstructor  = Vue.extend(Toast) // 返回一个“扩展实例构造器”
 
let myToast = (text,duration)=>{
    let toastDom = new ToastConstructor({
        el:document.createElement('div')    //将toast组件挂载到新创建的div上
    })
    document.body.appendChild( toastDom.$el )   //把toast组件的dom添加到body里
    
    toastDom.text = text;
    toastDom.duration = duration;
 
    // 在指定 duration 之后让 toast消失
    setTimeout(()=>{
        toastDom.isShow = false;  
    }, toastDom.duration);
}
export default myToast;
```

## 24.**什么是vue生命周期和[生命周期钩子函数](https://www.zhihu.com/search?q=生命周期钩子函数&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"article"%2C"sourceId"%3A97950650})？**

> **beforecreated**：在实例初始化之后，el 和 data 并未初始化（这个时期，this变量还不能使用，在data下的数据，和methods下的方法，watcher中的事件都不能获得到；）
> **created**:完成了 data 数据的初始化，el没有（这个时候可以操作vue实例中的数据和各种方法，但是还不能对"dom"节点进行操作；）
> **beforeMount**：完成了 el 和 data 初始化 //这里的el是虚拟的dom；
> **mounted** ：完成挂载，在这发起后端请求，拿回数据，配合路由钩子做一些事情（挂载完毕，这时dom节点被渲染到文档内，一些需要dom的操作在此时才能正常进行）
> **beforeUpdate**：是指view层数据变化前，不是data中的数据改变前触发；
> **update**：是指view层的数据变化之后，
> **beforeDestory**： 你确认删除XX吗？
> **destoryed** ：当前组件已被删除，清空相关内容
> **A、什么是[vue生命周期](https://www.zhihu.com/search?q=vue生命周期&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"article"%2C"sourceId"%3A97950650})？**
> Vue 实例从创建到销毁的过程，就是生命周期。也就是从开始创建、初始化数据、编译模板、挂载Dom→渲染、更新→渲染、卸载等一系列过程，我们称这是 Vue 的生命周期。
> **B、vue生命周期的作用是什么？**
> 它的生命周期有多个事件钩子,让我们在控制整个Vue实例的过程时更容易形成好的逻辑。
> **C、vue生命周期总共有几个阶段？**
> 它可以总共分为8个阶段：创建前/后, 载入前/后,更新前/后,销毁前/销毁后
> **D、第一次页面加载会触发哪几个钩子？**
> 第一次页面加载时会触发 beforeCreate, created, beforeMount, mounted 这几个钩子
> **E、DOM 渲染在 哪个周期中就已经完成？**
> DOM 渲染在 mounted 中就已经完成了。
> **F、简单描述每个周期具体适合哪些场景？**
> 生命周期钩子的一些使用方法： beforecreate : 可以在这加个loading事件，在加载实例时触发 created : 初始化完成时的事件写在这里，如在这结束loading事件，异步请求也适宜在这里调用 mounted : 挂载元素，获取到DOM节点 updated : 如果对数据统一处理，在这里写上相应函数 beforeDestroy : 可以做一个确认停止事件的确认框 nextTick : 更新数据后立即操作dom;

## 25.**vue更新数组时触发视图更新的方法**

```js
Vue.set    ==========Vue.set(target,key,value)这个方法主要是用于避开vue不能检测属性被添加的限制
Vue.set(array, indexOfItem, newValue)//indexOfItem指的索引
this.array.$set(indexOfItem, newValue)
Vue.set(obj, keyOfItem, newValue)
this.obj.$set(keyOfItem, newValue)
Vue.delete   这个方法主要用于避开vue不能检测到属性被删除；

Vue.delete(array, indexOfItem)
this.array.$delete(indexOfItem)
Vue.delete(obj, keyOfItem)
this.obj.$delete(keyOfItem)
```

## 26.**webpack的编译原理**

[webpack面试题汇总www.jianshu.com/p/bb1e76edc71e![img](https://pic3.zhimg.com/v2-98336c96095bbf88645f2a5de3ff9872_ipico.jpg)](https://link.zhihu.com/?target=https%3A//www.jianshu.com/p/bb1e76edc71e)

**webpack的作用**

> ①、依赖管理：方便引用第三方模块、让模块更容易复用，避免全局注入导致的冲突、避免重复加载或者加载不需要的模块。会一层一层的读取依赖的模块，添加不同的入口；同时，不会重复打包依赖的模块。
> ②、合并代码：把各个分散的模块集中打包成大文件，减少HTTP的请求链接数，配合UglifyJS（压缩代码）可以减少、优化代码的体积。
> ③、各路插件：统一处理引入的插件，babel编译ES6文件，TypeScript,eslint 可以检查编译期的错误。
> **一句话总结：**webpack 的作用就是处理依赖，模块化，打包压缩文件，管理插件。
> 一切皆为模块，由于webpack只支持js文件，所以需要用loader 转换为webpack支持的模块，其中plugin 用于扩张webpack 的功能，在webpack构建生命周期的过程中，在合适的时机做了合适的事情。

**webpack怎么工作的过程**

> ①解析配置参数，合并从shell(npm install 类似的命令)和webpack.config.js文件的配置信息，输出最终的配置信息；
> ②注册配置中的插件,让插件监听webpack构建生命周期中的事件节点，做出对应的反应；
> ③解析配置文件中的entry入口文件，并找出每个文件依赖的文件，递归下去；
> ④在递归每个文件的过程中，根据文件类型和配置文件中的loader找出对应的loader对文件进行转换；
> ⑤递归结束后得到每个文件最终的结果，根据entry 配置生成代码chunk(打包之后的名字)；
> ⑥输出所以chunk 到文件系统。

## 27.**vue等单页面应用及其优缺点**

**缺点：**

> 不支持低版本的浏览器，最低只支持到IE9；
> 不利于SEO的优化（如果要支持SEO，建议通过服务端来进行渲染组件）；
> 第一次加载首页耗时相对长一些；
> 不可以使用浏览器的导航按钮需要自行实现前进、后退。

**优点：**

> 无刷新体验,提升了用户体验；
> 前端开发不再以页面为单位，更多地采用组件化的思想，代码结构和组织方式更加规范化，便于修改和调整；
> API 共享，同一套后端程序代码不用修改就可以用于Web界面、手机、平板等多种客户端
> 用户体验好、快，内容的改变不需要重新加载整个页面。

## 28.**什么是vue的计算属性computed**

**计算属性是需要复杂的逻辑，可以用方法method代替**

```js
computed:{
    totalPrice(){
      return (this.good.price*this.good.count)*this.discount+this.deliver;
    }
  }
```

## **29.vue-cli提供的几种脚手架模板**

> vue-cli 的脚手架项目模板有browserify 和 webpack；

## 30.**组件中传递数据？**

```js
props：export default {
props: {

message: String //定义传值的类型<br>

},


//或者props:["message"]

data: {}

父组件调用子组件的方法：父组件   this.$refs.yeluosen.childMethod()


子组件向父组件传值并调用方法 $emit


组件之间：bus==$emit+$on
```

## 31.**vue-router实现路由懒加载（ 动态加载路由 ）**

![img](https://pic4.zhimg.com/80/v2-db295f51af4f814acd70dafe137b66f3_1440w.jpg)

## 32.**vue-router 的导航钩子,主要用来作用是拦截导航,让他完成跳转或取消。**

> **全局的:**前置守卫、后置钩子（beforeEach，afterEach）beforeResolve
> **单个路由独享的:**beforeEnter
> **组件级的:** beforeRouteEnter（不能获取组件实例 this）、beforeRouteUpdate、beforeRouteLeave
> 这是因为在执行[路由钩子函数beforRouteEnte](https://www.zhihu.com/search?q=路由钩子函数beforRouteEnte&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"article"%2C"sourceId"%3A97950650})r时候，组件还没有被创建出来；
> 先执行beforRouteEnter，再执行组件周期[钩子函数beforeCreat](https://www.zhihu.com/search?q=钩子函数beforeCreat&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"article"%2C"sourceId"%3A97950650})e，可以通过 next 获取组件的实例对象，如：next( (vm)=>{} )，参数vm就是组件的实例化对象。

## 33.**完整的 vue-router 导航解析流程**

> 1.导航被触发；
> 2.在失活的组件里调用beforeRouteLeave守卫；
> 3.调用全局beforeEach守卫；
> 4.在复用组件里调用beforeRouteUpdate守卫；
> 5.调用路由配置里的beforeEnter守卫；
> 6.解析异步路由组件；
> 7.在被激活的组件里调用beforeRouteEnter守卫；
> 8.调用全局beforeResolve守卫；
> 9.导航被确认；
> 10..调用全局的afterEach钩子；
> 11.DOM更新；
> 12.用创建好的实例调用beforeRouteEnter守卫中传给next的回调函数。

## 34.**vue-router如何响应 路由参数 的变化？**

**原来的组件实例会被复用。这也意味着组件的生命周期钩子不会再被调用。你可以简单地 watch (监测变化) $route 对象：**

```js
const User = {
  template: '...',
  watch: {
    '$route' (to, from) {
      // 对路由变化作出响应...
    }
  }
}


const User = {
  template: '...',
  watch: {
    '$route' (to, from) {
      // 对路由变化作出响应...
    }
  }
}
```

35.**vue-router的几种实例方法以及参数传递**

> name传递
> to来传递
> 采用url传参

## 36.**is的用法（用于动态组件且基于 DOM 内模板的限制来工作。）**

**is用来动态切换组件，DOM模板解析**

```js
<table> <tr is="my-row"></tr> </table>
```

## 37.**vuex是什么？怎么使用？哪种功能场景使用它？**

> 是什么：vue框架中状态管理:有五种，分别是 State、 Getter、Mutation 、Action、 Module
> 使用：新建一个目录store，
> 场景：单页应用中，组件之间的状态。音乐播放、登录状态、加入购物车

```js
vuex的State特性
A、Vuex就是一个仓库，仓库里面放了很多对象。其中state就是数据源存放地，对应于一般Vue对象里面的data
B、state里面存放的数据是响应式的，Vue组件从store中读取数据，若是store中的数据发生改变，依赖这个数据的组件也会发生更新
C、它通过mapState把全局的 state 和 getters 映射到当前组件的 computed 计算属性中

vuex的Getter特性
A、getters 可以对State进行计算操作，它就是Store的计算属性
B、 虽然在组件内也可以做计算属性，但是getters 可以在多组件之间复用
C、 如果一个状态只在一个组件内使用，是可以不用getters

vuex的Mutation特性
改变store中state状态的唯一方法就是提交mutation，就很类似事件。
每个mutation都有一个字符串类型的事件类型和一个回调函数，我们需要改变state的值就要在回调函数中改变。
我们要执行这个回调函数，那么我们需要执行一个相应的调用方法：store.commit。

Action 类似于 mutation，不同在于：Action 提交的是 mutation，而不是直接变更状态；
Action 可以包含任意异步操作，Action 函数接受一个与 store 实例具有相同方法和属性的 context 对象，
因此你可以调用 context.commit 提交一个 mutation，
或者通过 context.state 和 context.getters 来获取 state 和 getters。
Action 通过 store.dispatch 方法触发：eg。
store.dispatch('increment')

vuex的module特性
Module其实只是解决了当state中很复杂臃肿的时候，module可以将store分割成模块，
每个模块中拥有自己的state、mutation、action和getter
```