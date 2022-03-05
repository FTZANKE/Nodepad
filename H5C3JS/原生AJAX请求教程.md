# 原生AJAX请求教程

**`ajax`** 即 `Asynchronous Javascript And XML，AJAX` 不是一门的新的语言，而是对现有持术的综合利用。本质是在 HTTP 协议的基础上以异步的方式与服务器进行通信.

**异步**：指某段程序执行时不会阻塞其它程序执行，其表现形式为程序的执行顺序不依赖程序本身的书写顺序，相反则为同步。

### XMLHttpRequest 对象

浏览器内建对象，用于在后台与服务器通信(交换数据) ，由此我们便可实现对网页的部分更新，而不是刷新整个页面。

所有现代浏览器（IE7+、Firefox、Chrome、Safari 以及 Opera）均内建 XMLHttpRequest 对象。



```js
var xhr = new XMLHttpRequest();
```

> 老版本的 Internet Explorer （IE5 和 IE6）使用 ActiveX 对象：
> var xhr=new ActiveXObject("Microsoft.XMLHTTP");

如需将请求发送到服务器，我们使用 `XMLHttpRequest` 对象的 `open()` 和 `send()` 方法：



```js
var xhr = new XMLHttpRequest();
xhr.open('GET', 'ajax_info.json', true);
xhr.send();
```

| 方法                   | 描述                                                         |
| ---------------------- | ------------------------------------------------------------ |
| open(method,url,async) | 规定请求的类型、URL 以及是否异步处理请求。 **method**：请求的类型；GET 或 POST **url**：文件在服务器上的位置 **async**：true（异步）或 false（同步） |
| send(string)           | 将请求发送到服务器。string：仅用于 POST 请求                 |

### get请求

get请求参数需要放在url地址的参数中。并通过urlencode的方式传参，也就是`?`隔开url和参数，然后多个参数用`&`连接，参数格式为：`key=val`。

```js
var xhr = new XMLHttpRequest();
xhr.open("GET","/ajax.php?fname=Henry&lname=Ford",true);
xhr.send();
```

### post请求

post请求需要添加一个请求头，让后台知道我们请求的参数的格式，这样后台才能解析我们的数据。另外，传输的数据需要格式化到send方法中。

```js
var xhr = new XMLHttpRequest();
xhr.open("POST","/try/ajax/demo_post2.php",true);
xhr.setRequestHeader("Content-type","application/x-www-form-urlencoded");
xhr.send("fname=Henry&lname=Ford");
```

### 接受数据并处理数据

XMLHttpRequest对象的相关属性和事件

| 属性               | 说明                                                         |
| ------------------ | ------------------------------------------------------------ |
| status             | 200: "OK"                                                    |
| responseText       | 获得字符串形式的响应数据。                                   |
| responseXML        | 获得 XML 形式的响应数据。                                    |
| readyState         | 存有 XMLHttpRequest 的状态。请求发送到后台后，状态会从 0 到 4 发生变化。 **0**: 请求未初始化 **1**: 服务器连接已建立 **2**: 请求已接收 **3**: 请求处理中 **4**: 请求已完成，且响应已就绪 |
| onreadystatechange | 每当 readyState 属性改变时，就会调用该函数。                 |

开发人员，可以通过监听XMLHttpRequest对象的onreadystatechange事件，在事件的回调函数中判断readyState的状态，可以帮助我们进行对象请求结果的判断处理。

### 完整实例

- **完整的GET请求例子**：

```js
// get请求
var xhr = new XMLHttpRequest();

xhr.open('GET', '/api/user?id=333', true);
xhr.send();

xhr.onreadystatechange = function (e) {
  if (xhr.readyState == 4 && xhr.status == 200) {
    console.log(xhr.responseText);
  }
};
```

- **完整的POST请求例子**：

```js
var xhr = new XMLHttpRequest();

xhr.open('POST', '/api/user', true);
// POST请求需要设置此参数
xhr.setRequestHeader('Content-type', 'application/x-www-form-urlencoded')
xhr.send('name=33&ks=334');

xhr.onreadystatechange = function (e) {
  if (xhr.readyState == 4 && xhr.status == 200) {
    console.log(xhr.responseText);
  }
};
```

### 封装原生Ajax请求

封装get请求：

```js
/**
 * Ajax的Get请求辅助方法
 * @param {String} url  请求后台的地址
 * @param {Function} callback  请求成之后，返回数据成功，并且调用此方法，这个方法接受一个参数就是后台返回的数据。
 * @return undefined
 */
function ajaxGet(url, callback) {
  var xhr = new XMLHttpRequest();
  xhr.open('GET', url, true);
  xhr.send();

  xhr.onreadystatechange = function () {
    if (xhr.readyState == 4 && xhr.status == 200) {
      callback(xhr.responseText);
    }
  }
}

// 调用
ajaxGet('/user.json', function (data) {
  console.log(data);
});
```

封装post请求：

```js
function ajaxPost(url, data, callback) {
  var xhr = new XMLHttpRequest();
  xhr.open('POST', url, true);
  xhr.setRequestHeader('Content-type', 'application/x-www-form-urlencoded');
  xhr.send(data);

  xhr.onreadystatechange = function () {
    if (xhr.readyState == 4 && xhr.status == 200) {
      callback(xhr.responseText);
    }
  }
}

// 调用
ajaxPost('/api/user', 'id=9&com=aicoder', function (data) {
  // 后台返回的数据就是 字符串类型。要转成json，必须自己手动转换。
  var user = JSON.parse(data);
  console.log(user.id);
  console.log(user.com);
});
```