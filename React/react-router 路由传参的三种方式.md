# react-router 路由传参的三种方式

![]( https://pic2.zhimg.com/v2-88ee8a9a2ed5ceeb0f2861b3c1af0bf8_1440w.jpg?source=172ae18b)

## 1. params传参

优点：刷新页面，参数不丢失

缺点：1.只能传字符串，传值过多url会变得很长 2. 参数必须在路由上配置

#### 路由配置

```js
{ path: '/detail/:id/:name', component: Detail },
```

#### 路由跳转与获取路由参数

```js
import { useHistory,useParams } from 'react-router-dom';
const history = useHistory();
// 跳转路由   地址栏：/detail/2/zora
history.push('/detail/2/zora')
// 获取路由参数
const params = useParams()  
console.log(params) // {id: "2",name:"zora"}
```

## 2. search传参

优点：刷新页面，参数不丢失

缺点：只能传字符串，传值过多url会变得很长，获取参数需要自定义hooks

#### 路由配置

```js
{ path: '/detail', component: Detail },
```

#### 路由跳转与获取路由参数

```js
import { useHistory } from 'react-router-dom';
const history = useHistory();
// 路由跳转  地址栏：/detail?id=2
history.push('/detail?id=2')  
// 或者
history.push({pathname:'/detail',search:'?id=2'})

/**
* 自定义hooks用于获取路由参数
* IE11及以下浏览器 不支持浏览器内置的URLSearchParams API
**/
function useQuery() {
  return new URLSearchParams(useLocation().search);
}
const query = useQuery()
const id = query.get('id') //2
```

## 3. state传参🙌

优点：可以传对象

缺点： `<HashRouter>`刷新页面，参数丢失

#### 路由配置

```js
{ path: '/detail', component: Detail },
```

#### 路由跳转与获取路由参数

```js
import { useHistory,useLocation } from 'react-router-dom';
const history = useHistory();
const item = {id:1,name:"zora"}
// 路由跳转
history.push(`/user/role/detail`, { id: item });
// 参数获取
const {state} = useLocation()
console.log(state)  // {id:1,name:"zora"}
```

**重点 🏁** `<HashRouter>` 不支持 `location.key` 与 `location.state`，`<HashRouter>`通过`state`传递参数，刷新页面后参数丢失，官方建议使用`<BrowserRouter>`，`<BrowserRouter>`页面刷新参数也不会丢失。