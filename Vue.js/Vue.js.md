# Vue.js

## 1. Vue使用

```html
<!-- 开发版本 vue.js 包含完整的警告和调试模式 -->
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.12"></script>
<!-- 生产版本 vue.min.js 删除了警告 -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
```



## 2. Vue.js功能

1. 点击切换class

```html
<span class="static" v-bind:class="{ 'className-a' : isA, 'className-b': !isA}" @click="toggle">
```

```css
.className-a {
    color:red;
}
.className-b {
    color:black;
}
```

```javascript
new Vue({
    el: ".static",
    data: {
        isA: false
    },
    methods: {
        toggle: function () {
            this.isA = !this.isA;
        }
    }
})
```

## 3. Vue指令（简版）

1. v-text、v-html、

2. v-cloak、v-once、v-model

3. v-show，v-if：控制标签显示或隐藏
   语法:	

   - v-show="vue变量"

   - v-if="vue变量"

   原理：

   - v-show 用的display:none隐藏 (频繁切换使用)

   - v-if 直接从DOM树上移除

4. v-if、v-else、v-else-if

5. v-for: 用数据循环生成标签

   语法：

   - v-for="(值变量, 索引变量) in 目标结构"
   - v-for="值变量 in 目标结构"

   目标结构：可以遍历数组 / 对象 / 数字 / 字符串
   注意：v-for的临时变量名不能用到v-for范围外
   同级标签的key值不能重复

6. v-on：给元素绑定事件

   语法:

   - v-on:事件名="methods中的函数"

   - v-on:事件名="methods中的函数(实参)"
   - 简写: @事件名="methods中的函数"

   ```html
   <!-- vue指令:   v-on事件绑定-->
   <p>你要买商品的数量: {{count}}</p>
   <button v-on:click="addFn">增加1个</button>
   <button v-on:click="addCountFn(5)">一次加5件</button>
   <button @click="subFn">减少</button>
   <script>
       export default {
           // ...其他省略
           methods: {
               addFn(){ // this代表export default后面的组件对象(下属有data里return出来的属性)
                   this.count++
               },
               addCountFn(num){
                   this.count += num
               },
               subFn(){
                   this.count--
               }
           }
       }
   </script>
   ```

4. v-bind：给标签属性赋Vue变量

   1. 语法：`v-bind:属性名="vue变量"`
   2. 简写：`:属性名="vue变量"`

   ```html
   <!-- vue指令-v-bind属性动态赋值 -->
   <a v-bind:href="url">我是a标签</a>
   <img :src="imgSrc">
   ```

5. v-slot

6. v-pre

## 4.生命周期

**什么是 vue 生命周期?**
**答：**vue生命周期是指vue实例对象从创建之初到销毁的过程，vue所有功能的实现都是围绕其生命周期进行的，在生命周期的不同阶段调用对应的钩子函数实现组件数据管理和DOM渲染两大重要功能。

**生命周期视图：**

![](https://s3.bmp.ovh/imgs/2021/11/f3bb94399bfa2e02.jpg)

**8大生命周期函数：**

1. beforeCreate()
   在vue实例创建前执行，此时虚拟DOM没有创建，数据模型没有创建。所以在此处操作DOM不合适，获取数据也不合适。
2. created()
   在vue实例创建后执行，此时虚拟DOM没有创建，数据模型已经初始化。所以在此处操作DOM不合适，但获取数据合适。
3. beforeMount
   应用程序app和视图(虚拟DOM)挂载前执行。此时虚拟DOM已经创建，但没有编译和挂载。
4. mounted()
   应用程序app和视图(虚拟DOM)挂载后执行。此时虚拟DOM已经创建，并且进行了编译和挂载。
5. beforeUpdate()
   数据发生变化前执行。
6. updated()
   数据发生变化后执行。
7. beforeDestroy()
   实例销毁前执行
8. destoryed()
   实例销毁后执行

## 5. Vue.js 组件

组件(Component)可以扩展 HTML 元素，封装可重用的代码。

组件系统让我们可以用独立可复用的小组件来构建大型应用，几乎任意类型的应用的界面都可以抽象为一个组件树：

![img](https://www.runoob.com/wp-content/uploads/2017/01/components.png)

注册一个全局组件语法格式如下：

```vue
Vue.component(tagName, options)
```

tagName 为组件名，options 为配置选项。注册后，我们可以使用以下方式来调用组件：

```html
<tagName></tagName>
```

全局组件例子：

所有实例都能用全局组件。

```html
<div id="app">
    <x-template></x-template>
</div>
<script>
    // 注册
    Vue.component('x-template', {  // 形参写法：驼峰，-
        template: '<h1>自定义组件!</h1>'
    })
    // 创建根实例
    new Vue({
        el: '#app'
    })
</script>
```

局部组件例子：

在实例选项中注册局部组件，这样组件只能在这个实例中使用：

```html
<div id="app">
    <x-template></x-template>
</div>
<script>
    var Child = {
        template: '<h1>自定义组件!</h1>'
    }
    // 创建根实例
    new Vue({
        el: '#app',
        components: {
            // <runoob> 将只在父模板可用
            'x-template': Child
        }
    })
</script>
```

## 6. 事件

绑定和传参

绑定事件不添加()，默认会传递事件对象形参，添加()，需要明确传递$event。$event是vue提供的内置对象。

## 7. 指令（原版）

### [v-text](https://cn.vuejs.org/v2/api/#v-text)

- **预期**：`string`

- **详细**：

  更新元素的 `textContent`。如果要更新部分的 `textContent`，需要使用 `{{ Mustache }}` 插值。

- **示例**：

  ```
  <span v-text="msg"></span>
  <!-- 和下面的一样 -->
  <span>{{msg}}</span>
  ```

- **参考**：[数据绑定语法 - 插值](https://cn.vuejs.org/v2/guide/syntax.html#插值)

### [v-html](https://cn.vuejs.org/v2/api/#v-html)

- **预期**：`string`

- **详细**：

  更新元素的 `innerHTML`。**注意：内容按普通 HTML 插入 - 不会作为 Vue 模板进行编译**。如果试图使用 `v-html` 组合模板，可以重新考虑是否通过使用组件来替代。

  在网站上动态渲染任意 HTML 是非常危险的，因为容易导致 [XSS 攻击](https://en.wikipedia.org/wiki/Cross-site_scripting)。只在可信内容上使用 `v-html`，**永不**用在用户提交的内容上。

  在[单文件组件](https://cn.vuejs.org/v2/guide/single-file-components.html)里，`scoped` 的样式不会应用在 `v-html` 内部，因为那部分 HTML 没有被 Vue 的模板编译器处理。如果你希望针对 `v-html` 的内容设置带作用域的 CSS，你可以替换为 [CSS Modules](https://vue-loader.vuejs.org/en/features/css-modules.html) 或用一个额外的全局 `<style>` 元素手动设置类似 BEM 的作用域策略。

- **示例**：

  ```
  <div v-html="html"></div>
  ```

- **参考**：[数据绑定语法 - 插值](https://cn.vuejs.org/v2/guide/syntax.html#纯-HTML)

### [v-show](https://cn.vuejs.org/v2/api/#v-show)

- **预期**：`any`

- **用法**：

  根据表达式之真假值，切换元素的 `display` CSS property。

  当条件变化时该指令触发过渡效果。

- **参考**：[条件渲染 - v-show](https://cn.vuejs.org/v2/guide/conditional.html#v-show)

### [v-if](https://cn.vuejs.org/v2/api/#v-if)

- **预期**：`any`

- **用法**：

  根据表达式的值的 [truthiness](https://developer.mozilla.org/zh-CN/docs/Glossary/Truthy) 来有条件地渲染元素。在切换时元素及它的数据绑定 / 组件被销毁并重建。如果元素是 `<template>`，将提出它的内容作为条件块。

  当条件变化时该指令触发过渡效果。

  当和 `v-if` 一起使用时，`v-for` 的优先级比 `v-if` 更高。详见[列表渲染教程](https://cn.vuejs.org/v2/guide/list.html#v-for-with-v-if)

- **参考**：[条件渲染 - v-if](https://cn.vuejs.org/v2/guide/conditional.html)

### [v-else](https://cn.vuejs.org/v2/api/#v-else)

- **不需要表达式**

- **限制**：前一兄弟元素必须有 `v-if` 或 `v-else-if`。

- **用法**：

  为 `v-if` 或者 `v-else-if` 添加“else 块”。

  ```
  <div v-if="Math.random() > 0.5">
    Now you see me
  </div>
  <div v-else>
    Now you don't
  </div>
  ```

- **参考**：[条件渲染 - v-else](https://cn.vuejs.org/v2/guide/conditional.html#v-else)

### [v-else-if](https://cn.vuejs.org/v2/api/#v-else-if)

> 2.1.0 新增

- **类型**：`any`

- **限制**：前一兄弟元素必须有 `v-if` 或 `v-else-if`。

- **用法**：

  表示 `v-if` 的“else if 块”。可以链式调用。

  ```
  <div v-if="type === 'A'">
    A
  </div>
  <div v-else-if="type === 'B'">
    B
  </div>
  <div v-else-if="type === 'C'">
    C
  </div>
  <div v-else>
    Not A/B/C
  </div>
  ```

- **参考**：[条件渲染 - v-else-if](https://cn.vuejs.org/v2/guide/conditional.html#v-else-if)

### [v-for](https://cn.vuejs.org/v2/api/#v-for)

- **预期**：`Array | Object | number | string | Iterable (2.6 新增)`

- **用法**：

  基于源数据多次渲染元素或模板块。此指令之值，必须使用特定语法 `alias in expression`，为当前遍历的元素提供别名：

  ```
  <div v-for="item in items">
    {{ item.text }}
  </div>
  ```

  另外也可以为数组索引指定别名 (或者用于对象的键)：

  ```
  <div v-for="(item, index) in items"></div>
  <div v-for="(val, key) in object"></div>
  <div v-for="(val, name, index) in object"></div>
  ```

  `v-for` 的默认行为会尝试原地修改元素而不是移动它们。要强制其重新排序元素，你需要用特殊 attribute `key` 来提供一个排序提示：

  ```
  <div v-for="item in items" :key="item.id">
    {{ item.text }}
  </div>
  ```

  从 2.6 起，`v-for` 也可以在实现了[可迭代协议](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Iteration_protocols#可迭代协议)的值上使用，包括原生的 `Map` 和 `Set`。不过应该注意的是 Vue 2.x 目前并不支持可响应的 `Map` 和 `Set` 值，所以无法自动探测变更。

  当和 `v-if` 一起使用时，`v-for` 的优先级比 `v-if` 更高。详见[列表渲染教程](https://cn.vuejs.org/v2/guide/list.html#v-for-with-v-if)

  `v-for` 的详细用法可以通过以下链接查看教程详细说明。

- **参考**：

  - [列表渲染](https://cn.vuejs.org/v2/guide/list.html)
  - [key](https://cn.vuejs.org/v2/guide/list.html#key)

### [v-on](https://cn.vuejs.org/v2/api/#v-on)

- **缩写**：`@`

- **预期**：`Function | Inline Statement | Object`

- **参数**：`event`

- **修饰符**：

  - `.stop` - 调用 `event.stopPropagation()`。
  - `.prevent` - 调用 `event.preventDefault()`。
  - `.capture` - 添加事件侦听器时使用 capture 模式。
  - `.self` - 只当事件是从侦听器绑定的元素本身触发时才触发回调。
  - `.{keyCode | keyAlias}` - 只当事件是从特定键触发时才触发回调。
  - `.native` - 监听组件根元素的原生事件。
  - `.once` - 只触发一次回调。
  - `.left` - (2.2.0) 只当点击鼠标左键时触发。
  - `.right` - (2.2.0) 只当点击鼠标右键时触发。
  - `.middle` - (2.2.0) 只当点击鼠标中键时触发。
  - `.passive` - (2.3.0) 以 `{ passive: true }` 模式添加侦听器

- **用法**：

  绑定事件监听器。事件类型由参数指定。表达式可以是一个方法的名字或一个内联语句，如果没有修饰符也可以省略。

  用在普通元素上时，只能监听[**原生 DOM 事件**](https://developer.mozilla.org/zh-CN/docs/Web/Events)。用在自定义元素组件上时，也可以监听子组件触发的**自定义事件**。

  在监听原生 DOM 事件时，方法以事件为唯一的参数。如果使用内联语句，语句可以访问一个 `$event` property：`v-on:click="handle('ok', $event)"`。

  从 `2.4.0` 开始，`v-on` 同样支持不带参数绑定一个事件/监听器键值对的对象。注意当使用对象语法时，是不支持任何修饰器的。

- **示例**：

  ```
  <!-- 方法处理器 -->
  <button v-on:click="doThis"></button>
  
  <!-- 动态事件 (2.6.0+) -->
  <button v-on:[event]="doThis"></button>
  
  <!-- 内联语句 -->
  <button v-on:click="doThat('hello', $event)"></button>
  
  <!-- 缩写 -->
  <button @click="doThis"></button>
  
  <!-- 动态事件缩写 (2.6.0+) -->
  <button @[event]="doThis"></button>
  
  <!-- 停止冒泡 -->
  <button @click.stop="doThis"></button>
  
  <!-- 阻止默认行为 -->
  <button @click.prevent="doThis"></button>
  
  <!-- 阻止默认行为，没有表达式 -->
  <form @submit.prevent></form>
  
  <!--  串联修饰符 -->
  <button @click.stop.prevent="doThis"></button>
  
  <!-- 键修饰符，键别名 -->
  <input @keyup.enter="onEnter">
  
  <!-- 键修饰符，键代码 -->
  <input @keyup.13="onEnter">
  
  <!-- 点击回调只会触发一次 -->
  <button v-on:click.once="doThis"></button>
  
  <!-- 对象语法 (2.4.0+) -->
  <button v-on="{ mousedown: doThis, mouseup: doThat }"></button>
  ```

  在子组件上监听自定义事件 (当子组件触发“my-event”时将调用事件处理器)：

  ```
  <my-component @my-event="handleThis"></my-component>
  
  <!-- 内联语句 -->
  <my-component @my-event="handleThis(123, $event)"></my-component>
  
  <!-- 组件中的原生事件 -->
  <my-component @click.native="onClick"></my-component>
  ```

- **参考**：

  - [事件处理器](https://cn.vuejs.org/v2/guide/events.html)
  - [组件 - 自定义事件](https://cn.vuejs.org/v2/guide/components.html#监听子组件事件)

### [v-bind](https://cn.vuejs.org/v2/api/#v-bind)

- **缩写**：`:`

- **预期**：`any (with argument) | Object (without argument)`

- **参数**：`attrOrProp (optional)`

- **修饰符**：

  - `.prop` - 作为一个 DOM property 绑定而不是作为 attribute 绑定。([差别在哪里？](https://stackoverflow.com/questions/6003819/properties-and-attributes-in-html#answer-6004028))
  - `.camel` - (2.1.0+) 将 kebab-case attribute 名转换为 camelCase。(从 2.1.0 开始支持)
  - `.sync` (2.3.0+) 语法糖，会扩展成一个更新父组件绑定值的 `v-on` 侦听器。

- **用法**：

  动态地绑定一个或多个 attribute，或一个组件 prop 到表达式。

  在绑定 `class` 或 `style` attribute 时，支持其它类型的值，如数组或对象。可以通过下面的教程链接查看详情。

  在绑定 prop 时，prop 必须在子组件中声明。可以用修饰符指定不同的绑定类型。

  没有参数时，可以绑定到一个包含键值对的对象。注意此时 `class` 和 `style` 绑定不支持数组和对象。

- **示例**：

  ```
  <!-- 绑定一个 attribute -->
  <img v-bind:src="imageSrc">
  
  <!-- 动态 attribute 名 (2.6.0+) -->
  <button v-bind:[key]="value"></button>
  
  <!-- 缩写 -->
  <img :src="imageSrc">
  
  <!-- 动态 attribute 名缩写 (2.6.0+) -->
  <button :[key]="value"></button>
  
  <!-- 内联字符串拼接 -->
  <img :src="'/path/to/images/' + fileName">
  
  <!-- class 绑定 -->
  <div :class="{ red: isRed }"></div>
  <div :class="[classA, classB]"></div>
  <div :class="[classA, { classB: isB, classC: isC }]">
  
  <!-- style 绑定 -->
  <div :style="{ fontSize: size + 'px' }"></div>
  <div :style="[styleObjectA, styleObjectB]"></div>
  
  <!-- 绑定一个全是 attribute 的对象 -->
  <div v-bind="{ id: someProp, 'other-attr': otherProp }"></div>
  
  <!-- 通过 prop 修饰符绑定 DOM attribute -->
  <div v-bind:text-content.prop="text"></div>
  
  <!-- prop 绑定。“prop”必须在 my-component 中声明。-->
  <my-component :prop="someThing"></my-component>
  
  <!-- 通过 $props 将父组件的 props 一起传给子组件 -->
  <child-component v-bind="$props"></child-component>
  
  <!-- XLink -->
  <svg><a :xlink:special="foo"></a></svg>
  ```

  `.camel` 修饰符允许在使用 DOM 模板时将 `v-bind` property 名称驼峰化，例如 SVG 的 `viewBox` property：

  ```
  <svg :view-box.camel="viewBox"></svg>
  ```

  在使用字符串模板或通过 `vue-loader`/`vueify` 编译时，无需使用 `.camel`。

- **参考**：

  - [Class 与 Style 绑定](https://cn.vuejs.org/v2/guide/class-and-style.html)
  - [组件 - Props](https://cn.vuejs.org/v2/guide/components.html#通过-Prop-向子组件传递数据)
  - [组件 - `.sync` 修饰符](https://cn.vuejs.org/v2/guide/components-custom-events.html#sync-修饰符)

### [v-model](https://cn.vuejs.org/v2/api/#v-model)

- **预期**：随表单控件类型不同而不同。

- **限制**：

  - `<input>`
  - `<select>`
  - `<textarea>`
  - components

- **修饰符**：

  - [`.lazy`](https://cn.vuejs.org/v2/guide/forms.html#lazy) - 取代 `input` 监听 `change` 事件
  - [`.number`](https://cn.vuejs.org/v2/guide/forms.html#number) - 输入字符串转为有效的数字
  - [`.trim`](https://cn.vuejs.org/v2/guide/forms.html#trim) - 输入首尾空格过滤

- **用法**：

  在表单控件或者组件上创建双向绑定。细节请看下面的教程链接。

- **参考**：

  - [表单控件绑定](https://cn.vuejs.org/v2/guide/forms.html)
  - [组件 - 在输入组件上使用自定义事件](https://cn.vuejs.org/v2/guide/components-custom-events.html#将原生事件绑定到组件)

### [v-slot](https://cn.vuejs.org/v2/api/#v-slot)

- **缩写**：`#`

- **预期**：可放置在函数参数位置的 JavaScript 表达式 (在[支持的环境下](https://cn.vuejs.org/v2/guide/components-slots.html#解构插槽-Props)可使用解构)。可选，即只需要在为插槽传入 prop 的时候使用。

- **参数**：插槽名 (可选，默认值是 `default`)

- **限用于**

  - `<template>`
  - [组件](https://cn.vuejs.org/v2/guide/components-slots.html#独占默认插槽的缩写语法) (对于一个单独的带 prop 的默认插槽)

- **用法**：

  提供具名插槽或需要接收 prop 的插槽。

- **示例**：

  ```
  <!-- 具名插槽 -->
  <base-layout>
    <template v-slot:header>
      Header content
    </template>
  
    Default slot content
  
    <template v-slot:footer>
      Footer content
    </template>
  </base-layout>
  
  <!-- 接收 prop 的具名插槽 -->
  <infinite-scroll>
    <template v-slot:item="slotProps">
      <div class="item">
        {{ slotProps.item.text }}
      </div>
    </template>
  </infinite-scroll>
  
  <!-- 接收 prop 的默认插槽，使用了解构 -->
  <mouse-position v-slot="{ x, y }">
    Mouse position: {{ x }}, {{ y }}
  </mouse-position>
  ```

  更多细节请查阅以下链接。

- **参考**：

  - [组件 - 插槽](https://cn.vuejs.org/v2/guide/components-slots.html)
  - [RFC-0001](https://github.com/vuejs/rfcs/blob/master/active-rfcs/0001-new-slot-syntax.md)

### [v-pre](https://cn.vuejs.org/v2/api/#v-pre)

- **不需要表达式**

- **用法**：

  跳过这个元素和它的子元素的编译过程。可以用来显示原始 Mustache 标签。跳过大量没有指令的节点会加快编译。

- **示例**：

  ```
  <span v-pre>{{ this will not be compiled }}</span>
  ```

### [v-cloak](https://cn.vuejs.org/v2/api/#v-cloak)

- **不需要表达式**

- **用法**：

  这个指令保持在元素上直到关联实例结束编译。和 CSS 规则如 `[v-cloak] { display: none }` 一起用时，这个指令可以隐藏未编译的 Mustache 标签直到实例准备完毕。

- **示例**：

  ```
  [v-cloak] {
    display: none;
  }
  ```

  ```
  <div v-cloak>
    {{ message }}
  </div>
  ```

  

  不会显示，直到编译结束。

  

### [v-once](https://cn.vuejs.org/v2/api/#v-once)

- **不需要表达式**

- **详细**：

  只渲染元素和组件**一次**。随后的重新渲染，元素/组件及其所有的子节点将被视为静态内容并跳过。这可以用于优化更新性能。

  ```
  <!-- 单个元素 -->
  <span v-once>This will never change: {{msg}}</span>
  <!-- 有子元素 -->
  <div v-once>
    <h1>comment</h1>
    <p>{{msg}}</p>
  </div>
  <!-- 组件 -->
  <my-component v-once :comment="msg"></my-component>
  <!-- `v-for` 指令-->
  <ul>
    <li v-for="i in list" v-once>{{i}}</li>
  </ul>
  ```

- **参考**：

  - [数据绑定语法- 插值](https://cn.vuejs.org/v2/guide/syntax.html#插值)
  - [组件 - 对低开销的静态组件使用 `v-once`](https://cn.vuejs.org/v2/guide/components-edge-cases.html#通过-v-once-创建低开销的静态组件)
  

## 8. 定义组件

1. 全局的Vue.component中	

```js
Vue.component('com1', {
    template: `<div>111111</div>`
})
```



2. 局部vm实例的components中
```html
<script type="text/x-template" id="tem">
<div>222222</div>
</script>
<script>
    new Vue({
        el: '.app',
        data: {
        },
        components: {
            // 组件定义2
            'com2': {
                template: `#tem`
            }
        }
    })
</script>
```


##  9. 组件传值

1. **`props/$emit`**

   父组件向子组件传递数据，使用props属性；子组件向父组件中传递数据，在子组件中使用$emit派发事件，父组件中使用v-on 监听事件；缺点：组件嵌套层次多的话，传递数据比较麻烦。

   例子：

   ## 父组件向子组件中传递数据

   一个组件默认可以拥有任意数量的 prop，任何值都可以传递给任何 prop。
   当一个值传递给一个 prop attribute 的时候，它就变成了那个组件实例的一个属性（例如下面的 subNum），通过属性名，我们就能够在组件实例中访问这个值。
   父组件向子组件中传递数据，可以在子组件中通过设置props属性来接收传递过来的数据。

 ```html
 <div id="app">
     <div>{{num}}</div>
     <!-- 将父组件中的num，传递给子组件中的sub-num -->
     <blog-count :sub-num="num" :sub-user="user"></blog-count>
 </div>
 <script type="text/x-template" id="tem">
   	<div>
   		<p>这是从父组件传进来的数字：{{subNum}}</p>
   		<p>这是从父组件传进来的对象：{{subUser.name}}-{{subUser.age}}</p>
     </div>
 </script>
 <script>
     const blogCount = {
         //子组件中通过props属性接收父组件传递过来的数据
         props:["subNum","subUser"],
         template:"#tem",
     }
     var vm = new Vue({
         el:"#app",
         data:{
             num:2,
             user:{
                 name:"zhangsan",
                 age:18
             }
         },
         components:{
             blogCount
         }
     })
 </script>
 <!-- 第二个例子 -->
 <div class="app">
     <div is='componentA' v-bind:dataa="msg">
         <template v-slot:cont>
             <div>01</div>
         </template>
     </div>
 </div>
 <script type="text/x-template" id="tmp">
 		<div>子组件,{{content}}
 			<slot name="cont"></slot>
 			<div>{{dataa}}</div>
     </div>
 </script>
 <script>
     new Vue({
         el: '.app',
         data: {
             msg: 'My name is AHao.'
         },
         components: {
             componentA: {
                 template: '#tmp',
                 props:  ['dataa'], // props写法: ['dataa'],{dataa:''},{dataa:传过来的类型}
                 data() {
                     return {
                         content: '子组件内容'
                     }
                 },
             }
         }
     })
 </script>
 
 ```

   ## 子组件向父组件传递数据

   - **子组件向父组件传递数据，通过 $emit派发事件，父组件中通过 v-on(简写：@) 接收该事件，拿到传递的数据**。
   - 语法：`$emit(eventName，data)`，第一个参数为事件名称，需要跟父组件中 v-on 监听的事件名称一致；第二个参数为要传递的数据。
   - 记住**全局注册的行为必须在根 Vue 实例 (通过 `new Vue`) 创建之前发生**。[这里](https://github.com/chrisvfritz/vue-enterprise-boilerplate/blob/master/src/components/_globals.js)有一个真实项目情景下的示例。[^官方]

  ```html
  <div id="app">
      <div>{{num}}</div>
      <!-- 通过 v-on 监听事件-->
      <blog-count @countchange="changeHandle"></blog-count>
  </div>
  <script type="text/x-template" id="tmp">
   		<button @click="clickHandle">点击接收子组件传递过来的数据</button>
  </script>
  <script>
      const blogCount = {
          template: `#tmp`,
          data() {
              return { num: 6 }
          },
          methods: {
              clickHandle() {
                  //使用 $emit派发事件
                  this.$emit("countchange", this.num);
              }
          }
      }
      var vm = new Vue({
          el: "#app",
          data: {
              num: 0
          },
          methods: {
              changeHandle(data) {
                  this.num = data;
              }
          },
          components: {
              blogCount
          }
      })
  </script>
  <!-- 第二个例子 -->
  <div class="app">
      {{msg}}
      <div is="myChild" @say="say"></div>
  </div>
  <script type="text/x-template" id="child">
  		<div>全局组件:{{msg}}</div>
  </script>
  <script>
      // 全局组件
      Vue.component('myChild', {
          template: '#child',
          data() {
              return {
                  msg: 'My name is AHao.'
              }
          },
          mounted() {
              this.$emit('say', this.msg)
          },
      })
      // 父组件
      let par = new Vue({
          el: '.app',
          data: { msg: '' },
          methods: {
              say(rest) {
                  this.msg = rest;
                  console.log(this.msg);
                  console.log(rest);
              }
          }
      });
  </script>
  
  ```

## 10. filter过滤器

```js
Vue.filter('filterName',val=>'http'+val) // 全局

filters:{ //局部
  filterName(val){
    return 'http'+val;
  }
}

// <img :src="car.img|filterName" /> //调用时这样写

```

## 11.[将原生事件绑定到组件](https://cn.vuejs.org/v2/guide/components-custom-events.html#将原生事件绑定到组件) 

**native修饰符**

你可能有很多次想要在一个组件的根元素上直接监听一个原生事件。这时，你可以使用 `v-on` 的 `.native` 修饰符：

```html
<base-input v-on:focus.native="onFocus"></base-input>
```

## 12.导航守卫

全局前置守卫 `router.beforeEach`

全局解析守卫 `router.beforeResolve`

全局后置钩子 `router.afterEach`

路由独享的守卫 `beforeEnter`

组件内的守卫

- `beforeRouteEnter`  进入组件
- `beforeRouteUpdate` (2.2 新增)   修改数据
- `beforeRouteLeave` 离开组件

### 导航解析流程

1. 导航被触发。
2. 在失活的组件里调用 `beforeRouteLeave` 守卫。
3. 调用全局的 `beforeEach` 守卫。
4. 在重用的组件里调用 `beforeRouteUpdate` 守卫 (2.2+)。
5. 在路由配置里调用 `beforeEnter`。
6. 解析异步路由组件。
7. 在被激活的组件里调用 `beforeRouteEnter`。
8. 调用全局的 `beforeResolve` 守卫 (2.5+)。
9. 导航被确认。
10. 调用全局的 `afterEach` 钩子。
11. 触发 DOM 更新。
12. 调用 `beforeRouteEnter` 守卫中传给 `next` 的回调函数，创建好的组件实例会作为回调函数的参数传入。

### 钩子顺序：

**当点击切换路由时：**beforeRouteLeave-->beforeEach-->beforeEnter-->beforeRouteEnter-->beforeResolve-->afterEach-->beforeCreate-->created-->beforeMount-->mounted-->beforeRouteEnter的next的回调

**当路由更新时：**beforeRouteUpdate

## 13.beforeRouteEnter守卫不能访问this的原因?

因为守卫在导航确认前被调用，因此即将登场的新组件还没被创建。

可以通过传一个回调给 `next`来访问组件实例。在导航被确认的时候执行回调，并且把组件实例作为回调方法的参数。

```js
beforeRouteEnter (to, from, next) {
    next(vm => { // `vm`相当于`this`,这时候 this = undefined
        // 通过 `vm` 访问组件实例
    })
}
```

注意 `beforeRouteEnter` 是支持给 `next` 传递回调的唯一守卫。对于 `beforeRouteUpdate` 和 `beforeRouteLeave` 来说，`this` 已经可用了，所以**不支持**传递回调，因为没有必要了。

