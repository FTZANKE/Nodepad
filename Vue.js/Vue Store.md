# Vue Store

## store

#### state mutations getter,actions

- state ：对数据的全局存储
- getter： 可以理解为computed ，对数据进行计算
- mutations ：对数据的同步更改
- actions：对数据的异步更改



```components
    mounted: {
        console.log(this.$store);
        /*payLoad 所有的参数对象 {a:aa,b:bb}*/
        /*调用mutation方法*/
        this.$store.commit("mutationsFun", payLoad)
           
        /*调用action 方法*/
        this.$store.dispatch("ActionsFun",payLoad)
    },
    computed: {
        counter() {
            return this.$store.state.count;
        }
    } 
```

#### 辅助函数

- 引入



```swift
 import
{
    mapState,
    mapGetter,
    mapMutation,
    mapAction
 }
```

- 使用



```jsx
[
   computed: {
       ...mapState(['count']) /*映射同名的对象：count */
       ...mapState({
           counter: 'count' /*映射不同名字的对象：count--->counter */
       })
       ...mapState({
           counter: (state) => state.count /*映射不同名字的对象：count--->counter */
       })

       ...mapGetters(["fullName"])
   }

   methods：{
       /*将actions，mutations里面的方法映射到对应的methods,以便在组件里面可以直接应用*/
       ...mapActions(['ActionsFuncName'])
       ...mapMutations(['MutationsFuncName'])
   }
]
```

#### modules

- modules 子state ，加入对应的作用域
- 子模块中的方法参数state 作用域为子模块



```csharp
modules:{
 /*子模块的根目录 */
    AModule: {
        getters: {
            textPlus(state, getters, rootState) {
                return state.AModuleStateVar + rootStore.textVar + rootStore.BModule.StateVar /*可以取得root变量 可以去取其他模块 */
            }
        },
        actions: {
            add(state, commit, rootState) {
                /*commit 默认在本模块中找对应的mutation */
                /*设置root :true */
                commit("mutationName", 参数对象， {
                    root: true
                });
                commit("OtherMudule/mutationName", 参数对象， {
                    root: true
                })
            }
        }
    }
}
```

- 调用子module



```kotlin
    mounted:
    {
        this.["AModuleName/ActionFunName"](参数对象)
        this.["AModuleName/MutationFunName"](参数对象)
    }
    computed: {
        textA() {
            return this.$store.state.AModule.stateVar
        }
        ...mapState({
            counter: (state) => state.AModule.stateVar /*映射不同名字的对象：count--->counter */
        })

        ...mapGetters(["AModuleName/GetterFunName"])
        ...mapGetters({
            rootGetterFunName: 'rootGettterFunName',
            AModuleGetterFunName: "AModuleName/GetterFunName"
        })
    }

    methods： {
        /*将actions，mutations里面的方法映射到对应的methods,以便在组件里面可以直接应用*/
        ...mapActions(['AModule/ActionsFuncName']) /*namespaced:true 需要调用namespace的名字*/
        ...mapActions([' AModuleNameActionsFuncName']) /*namespaced:false 需要调用namespace的名字*/
        ...mapMutations(['AModule_MutationsFuncName']) /*直接调用子模块的名字 */ /*namespaced:false 需要调用namespace的名字*/
        /*子模块中的方法参数state 作用域为子模块 */
        ...mapMutations(['AModuleName/AModule_MutationsFuncName']) /*namespaced:true 需要调用namespace的名字*/
    } 
```

#### 动态注册模块功能



```jsx
import store from "./store";
/*等同于模块AModule*/
store.registerModule("CModule", {
    state:...
    action:...
})
store.unRegisterModule("CModule")
```

#### 开发过程中store会更新里面的action等内容，开发页面则会不断刷新



```tsx
// store.js
import Vue from 'vue'
import Vuex from 'vuex'
import mutations from './mutations'
import moduleA from './modules/a'

Vue.use(Vuex)

const state = {
    ...
}

const store = new Vuex.Store({
    state,
    mutations,
    modules: {
        a: moduleA
    }
})

if (module.hot) {
    // 使 action 和 mutation 成为可热重载模块
    module.hot.accept(['./mutations', './modules/a', ...], () => {
        // 获取更新后的模块
        // 因为 babel 6 的模块编译格式问题，这里需要加上 `.default`
        const newMutations = require('./mutations').default
        const newModuleA = require('./modules/a').default
            .
            .
            .
        // 加载新模块
        store.hotUpdate({
            mutations: newMutations,
            modules: {
                a: newModuleA
            }
            .
            .
            .
        })
    })
}
```

#### Store API



```store
/* */

store.watch((state)=?state.count+1,(newCounter)=>{

})

store.subscribe( (mutation,state)=>{
    /*监听哪一个mutaion 被调用了*/
    console.log(mutation.type)/*调用的是哪一个mutation function*/
    console.log(mutation.payLoad)/*mutation 传入的参数*/
})

store.subscribeAction( (action,state)=>{
    /*action 被调用了*/
    console.log(action.type)/*调用的是哪一个action function*/
    console.log(action.payLoad)/*action 传入的参数*/
})
```

#### Store Plugin

- plugin 可以顺序执行，每一个plugin就是一个方法



```css
new Vuex.Store({
    strict: true,
    plugins: [(store) => {
        console.log("my plugin invorked")
    }]
})
```

#### 其他



```css
new Vuex.Store({
    strict:process.env.NoDE_ENV==='development'//production
}) 
```

#### 链接官网

[https://vuex.vuejs.org/zh/guide/hot-reload.html](https://links.jianshu.com/go?to=https%3A%2F%2Fvuex.vuejs.org%2Fzh%2Fguide%2Fhot-reload.html)