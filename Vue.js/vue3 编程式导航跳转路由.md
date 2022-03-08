## vue3 编程式导航跳转路由    

### 引入 vue-router

```js
import { useRouter , useRoute } from 'vue-router'
```

### setup中使用

```js
setup(){
    const $router=useRouter()
    const $route=useRoute()

    // 对象 $router.push()可以向history对象添加新记录
    $router.push({ path: 'home' })

    // 命名的路由
    $router.push({ name: 'user', params: { userId: '123' }})

    // 带查询参数，变成 /register?plan=private
    $router.push({ path: 'register', query: { plan: 'private' }})

    //$router.replace 替换当前路由不会向history对象添加记录 与push()基本一致
    $router.replace({ path: 'home' })
}
```