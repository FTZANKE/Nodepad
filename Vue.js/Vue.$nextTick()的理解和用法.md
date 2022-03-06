# Vue.$nextTick()的理解和用法

nextTick回顾，这一切都是为了自己不被$nextTink(() => {})所抛弃！！！

# vm.$nextTick([callback])

- 参数：

{Function} [callback]

- 用法
  将回调延迟到下次 DOM 更新循环之后执行。在修改数据之后立即使用它，然后等待 DOM 更新。它跟全局方法`Vue.nextTick`一样，不同的是回调的 this 自动绑定到调用它的实例上。

示例：



```jsx
Vue.nextTick
// 修改数据
vm.msg = 'Hello'
// DOM 还没有更新
Vue.nextTick(function () {
  // DOM 更新了
})

// 作为一个 Promise 使用 (2.1.0 起新增，详见接下来的提示)
Vue.nextTick()
  .then(function () {
    // DOM 更新了
  })
```



```jsx
this.$nextTick(() => {})

new Vue({
  // ...
  methods: {
    // ...
    example: function () {
      // 修改数据
      this.message = 'changed'
      // DOM 还没有更新
      this.$nextTick(function () {
        // DOM 现在更新了
        // `this` 绑定到当前实例
        this.doSomethingElse()
      })
    }
  }
})
```

# 异步更新队列

Vue 在更新 DOM 时是异步执行的，当你设置 vm.someData = 'new value'，该组件不会立即重新渲染，可以在数据变化之后立即使用 Vue.nextTick(callback)。这样回调函数将在 DOM 更新完成后被调用。



```php
<div id="example">{{message}}</div>
var vm = new Vue({
  el: '#example',
  data: {
    message: '123'
  }
})
vm.message = 'new message' // 更改数据
vm.$el.textContent === 'new message' // false
Vue.nextTick(function () {
  vm.$el.textContent === 'new message' // true
})
```

在组件内使用 vm.$nextTick() 实例方法特别方便，因为它不需要全局 Vue，并且回调函数中的 this 将自动绑定到当前的 Vue 实例上：



```jsx
Vue.component('example', {
  template: '<span>{{ message }}</span>',
  data: function () {
    return {
      message: '未更新'
    }
  },
  methods: {
    updateMessage: function () {
      this.message = '已更新'
      console.log(this.$el.textContent) // => '未更新'
      this.$nextTick(function () {
        console.log(this.$el.textContent) // => '已更新'
      })
    }
  }
})
```

图解：



![img](https://upload-images.jianshu.io/upload_images/24655711-57088bb169327b5f.png?imageMogr2/auto-orient/strip|imageView2/2/w/423/format/webp)

nextTick.png

官网说：

> 只要侦听到数据变化，Vue 将开启一个队列，并缓冲在同一事件循环中发生的所有数据变更。如果同一个 watcher 被多次触发，只会被推入到队列中一次。

按照事件循环解释为：

- 第一个 tick（图例中第一个步骤，即'本次更新循环'）：
  首先修改数据，这是同步任务。同一事件循环的所有的同步任务都在主线程上执行，形成一个执行栈，此时还未涉及 DOM 。
  Vue 开启一个异步队列，并缓冲在此事件循环中发生的所有数据改变。如果同一个 watcher 被多次触发，只会被推入到队列中一次。
- 第二个 tick（图例中第二个步骤，即'下次更新循环'）：
  同步任务执行完毕，开始执行异步 watcher 队列的任务，更新 DOM 。Vue 在内部尝试对异步队列使用原生的 Promise.then 和 MessageChannel 方法，如果执行环境不支持，会采用 setTimeout(fn, 0) 代替。
- 第三个 tick（图例中第三个步骤）：
  此时就是文档所说的
  下次 DOM 更新循环结束之后
  此时通过 Vue.nextTick 获取到改变后的 DOM 。通过 setTimeout(fn, 0) 也可以同样获取到。

#### 简单总结事件循环：

同步代码执行 -> 查找异步队列，推入执行栈，执行Vue.nextTick[事件循环1] ->查找异步队列，推入执行栈，执行Vue.nextTick[事件循环2]...
总之，异步是单独的一个tick，不会和同步在一个 tick 里发生，也是 DOM 不会马上改变的原因。

# 用途

**created、mounted**
需要注意的是，在 created 和 mounted 阶段，如果需要操作渲染后的试图，也要使用 nextTick 方法。
官方文档说明：

> 注意 mounted 不会承诺所有的子组件也都一起被挂载。如果你希望等到整个视图都渲染完毕，可以用 vm.$nextTick 替换掉 mounted



```jsx
mounted: function () {
    this.$nextTick(function () {
        // Code that will run only after the
        // entire view has been rendered
    })
}
```

## 其他应用场景

例子1：

点击按钮显示原本以 v-show = false 隐藏起来的输入框，并获取焦点。



```js
showsou(){
    this.showit = true //修改 v-show
    document.getElementById("keywords").focus()  //在第一个 tick 里，获取不到输入框，自然也获取不到焦点
}
```

修改为：



```js
showsou(){
    this.showit = true
    this.$nextTick(function () {
        // DOM 更新了
        document.getElementById("keywords").focus()
    })
}
```

例子2：

点击获取元素宽度。



```jsx
<div id="app">
    <p ref="myWidth" v-if="showMe">{{ message }}</p>
    <button @click="getMyWidth">获取p元素宽度</button>
</div>

getMyWidth() {
    this.showMe = true;
    //this.message = this.$refs.myWidth.offsetWidth;
    //报错 TypeError: this.$refs.myWidth is undefined
    this.$nextTick(()=>{
        //dom元素更新后执行，此时能拿到p元素的属性
        this.message = this.$refs.myWidth.offsetWidth;
  })
}
```

例子3：

使用 swiper 插件通过 ajax 请求图片后的滑动问题。

## 实例理解 nextTick 应用



```vue
<template>
    <div>
        <ul>
            <li class="example" v-for="item in list1">{{item}}</li>
        </ul>
        <ul>
            <li class="example" v-for="item in list2">{{item}}</li>
        </ul>
        <ol>
            <li class="example" v-for="item in list3">{{item}}</li>
        </ol>
        <ol>
            <li class="example" v-for="item in list4">{{item}}</li>
        </ol>
        <ol>
            <li class="example" v-for="item in list5">{{item}}</li>
        </ol>
    </div>
</template>
<script type="text/javascript">
export default {
    data() {
        return {
            list1: [],
            list2: [],
            list3: [],
            list4: [],
            list5: []
        }
    },
    created() {
        this.composeList12()
        this.composeList34()
        this.composeList5()
        this.$nextTick(function() {
            // DOM 更新了
            console.log('finished test ' + new Date().toString(),document.querySelectorAll('.example').length)
        })
    },
    methods: {
        composeList12() {
            let me = this
            let count = 10000

            for (let i = 0; i < count; i++) {
                this.$set(me.list1, i, 'I am a 测试信息～～啦啦啦' + i)
            }
            console.log('finished list1 ' + new Date().toString(),document.querySelectorAll('.example').length)

            for (let i = 0; i < count; i++) {
                this.$set(me.list2, i, 'I am a 测试信息～～啦啦啦' + i)
            }
            console.log('finished list2 ' + new Date().toString(),document.querySelectorAll('.example').length)

            this.$nextTick(function() {
                // DOM 更新了
                console.log('finished tick1&2 ' + new Date().toString(),document.querySelectorAll('.example').length)
            })
        },
        composeList34() {
            let me = this
            let count = 10000

            for (let i = 0; i < count; i++) {
                this.$set(me.list3, i, 'I am a 测试信息～～啦啦啦' + i)
            }
            console.log('finished list3 ' + new Date().toString(),document.querySelectorAll('.example').length)

            this.$nextTick(function() {
                // DOM 更新了
                console.log('finished tick3 ' + new Date().toString(),document.querySelectorAll('.example').length)
            })

            setTimeout(me.setTimeout1, 0)
        },
        setTimeout1() {
            let me = this
            let count = 10000

            for (let i = 0; i < count; i++) {
                this.$set(me.list4, i, 'I am a 测试信息～～啦啦啦' + i)
            }
            console.log('finished list4 ' + new Date().toString(),document.querySelectorAll('.example').length)

            me.$nextTick(function() {
                // DOM 更新了
                console.log('finished tick4 ' + new Date().toString(),document.querySelectorAll('.example').length)
            })
        },
        composeList5() {
            let me = this
            let count = 10000

            this.$nextTick(function() {
                // DOM 更新了
                console.log('finished tick5-1 ' + new Date().toString(),document.querySelectorAll('.example').length)
            })

            setTimeout(me.setTimeout2, 0)
        },
        setTimeout2() {
            let me = this
            let count = 10000

            for (let i = 0; i < count; i++) {
                this.$set(me.list5, i, 'I am a 测试信息～～啦啦啦' + i)
            }
            console.log('finished list5 ' + new Date().toString(),document.querySelectorAll('.example').length)

            me.$nextTick(function() {
                // DOM 更新了
                console.log('finished tick5 ' + new Date().toString(),document.querySelectorAll('.example').length)
            })
        }
    }
}
</script>
```

感谢：[将臣](https://links.jianshu.com/go?to=https%3A%2F%2Fsegmentfault.com%2Fa%2F1190000012861862)

