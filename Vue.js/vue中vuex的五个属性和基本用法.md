## vue中vuex的五个属性和基本用法

VueX 是一个专门为 Vue.js 应用设计的状态管理构架，统一管理和维护各个vue组件的可变化状态(你可以理解成 vue 组件里的某些 data )。

Vuex有五个核心概念：

　　`state`, `getters`, `mutations`, `actions`, `modules`。

　　1. state：vuex的基本数据，用来存储变量

　　 2. geeter：从基本数据(state)派生的数据，相当于state的计算属性

　　 3. mutation：提交更新数据的方法，必须是同步的(如果需要异步使用action)。每个 mutation 都有一个字符串的 事件类型 (type) 和 一个 回调函数 (handler)。

　　 回调函数就是我们实际进行状态更改的地方，并且它会接受 state 作为第一个参数，提交载荷作为第二个参数。

　　 4. action：和mutation的功能大致相同，不同之处在于 ==》1. Action 提交的是 mutation，而不是直接变更状态。 2. Action 可以包含任意异步操作。

　　 5. modules：模块化vuex，可以让每一个模块拥有自己的state、mutation、action、getters,使得结构非常清晰，方便管理。