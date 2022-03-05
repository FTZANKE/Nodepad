# Fetch()方法介绍

在`javascript`中，所有的代码都是以单线程的方式执行的，因而网络请求，浏览器事件等操作都需要使用异步的方法。

## XMLRequest(XHR)

```javascript
var xhr=new XMLHttpRequest()
xhr.open('get','/some/url',true)
xhr.responseType = 'json';
xhr.send()
xhr.onreadystatechange=function(){
    if(xhr.status=200&&xhr.readyState==4){
        console.log(xhr.response)
    }
}
```

### 告别XMLHttpRequest

fetch()方法与XMLHttpRequest类似，fetch也可以发起ajax请求，但是与XMLHttpRequest不同的是，fetch方式使用Promise，相比较XMLHttpRequest更加的简洁。

如果你还不了解Promise，需要先补充，关于Promise的一些知识。

### Promise

Promise是进行异步操作的一种解决方案，比传统的处理方法(回调函数/处理事件)更加合理，ES6将其写入了语言标准,统一了语法，原生提供了Promise。 Promise可以想象成一个装有各种结果的容器，里面装有某个时间返回来的结果，你可以在需要的时候拿取它并进行一些事件处理。

从上边的介绍，大概可以看出Promise相比较于传统的异步操作的一个优势在于不用在异步操作的同时进行事件的处理，更加的合理性。

### Promise使用方法

在ES6中规定,Promise对象是一个构造函数,用来生成Promise实例。

```javascript
const promist = new Promise(function(resolve,reject){
    if(/*异步操作成功*/){
        resolve(value);
    }else{
        reject(error);
    }
})

```

#### promise实例

```javascript
promise 处理异步任务
var x = Math.random()
new Promise(function (resolve, reject) {
    // 如果成功
    if (x > 0.5) {
        resolve(x)
    } else {
        // 失败
        reject('错误！')
    }
}).then(function (data) {
    //获取的数据
    console.log(data);
}).catch(function (err) {
    //失败
    console.log(err);
})
```



**esolve**在异步操作成功时调用，并将异步操作的结果，作为参数传递出去

**reject**在异步操作失败时调用，并将异步操作错误结果，作为参数传递出去

`Promise`实例生成后可以用then()方法操作成功/失败的回调函数

```javascript
promise.then(function(value){
    <!-- 成功的回调处理 -->
},function(error){
    <!-- 失败的回调处理 -->
})
```

## 基本的fetch请求

简单的了解了Promise后我们就可以对fetch()方法有一个很好的认识了，**fetch**是全局量window的一个方法，第一个参数为URL。

```javascript
// url (必须), options (可选)
fetch('/some/url', {
    method: 'get'
}).then(function(response) {

}).catch(function(err) {
    // 出错了;等价于 then 的第二个参数,但这样更好用更直观 :(

});

```



#### 参数

url参数是必须要填写的，option可选，设置fetch调用时的Request对象，如method、headers等
比较常用的Request对象有：

Options：

1. methods：HTTP请求方法，默认为get
2. body：HTTP的请求参数
3. headers：HTTP的请求头，默认为{}
4. credentials：默认为omit，忽略的意思，不带cookie；还有两个参数：same-orgin意思就是同源请求带cookie；include表示无论跨域还是同源都会带有cookie。
5. status：HTTP返回的状态码，范围在100-599之间
6. statusText：服务器返回的状态文字描述
7. ok：如果状态码是以2开头，则为true

- body：返回体，如下是处理返回的一些方法
- text()：将返回体处理成字符串类型
- json()：返回结果和JSON.parse(rersponseText)一样
- blob()：返回一个Blob，Blob对象是一个不可更改的类文件的二进制数据
- arrayBuffer()
- formData()

如提交示例如下：

```javascript
fetch('/users.json', {
    method: 'POST', 
    //json格式
    body: JSON.stringify({
        email: 'huang@163.com'
        name: 'jim'
    }) 
    //text格式
    body: (
    email: 'huang@163.com'
    name: 'jim'
    )

}).then(function() { /* 处理响应 */ });
//headers: {
//     "Content-Type": 'application/json;charset="utf-8'
// }
```

### Response响应

fetch方法的then会接收一个Response实例，值得注意的是fetch方法的第二个then接收的才是后台传过来的真正的数据，一般第一个then对数据进行处理等。

例如fetch处理JSON响应时 回调函数有一个json()方法，可以将原始数据转换为json对象


```javascript
fetch('/some/url', { method: 'get', })
// 第一个then  设置请求的格式
    .then(e => e.json())
// 第二个then 处理回调
    .then((data) => {
    <!-- data为真正数据 -->
}).catch(e => console.log("Oops, error", e))

```

## 使用fetch请求发送cookie

```javascript
fetch(url,{
    credentials:"include"
})
```



使用fetch方法请求数据更加的简单，语法简洁，数据处理过程更加的清晰，基于Promise实现。



