[toc]

# BOM

1. BOM：**浏览器对象模型 Browser Object Model**，是Javascript的重要组成部分。它提供了一系列对象用于与浏览器窗口进行交互，这些对象通常统称为BOM。
2. 常见的BOM对象有哪些?

| 对象      | 作用                                                         |
| --------- | ------------------------------------------------------------ |
| window    | 代表整个浏览器窗口（window是BOM中的一个对象，并且是顶级的对象） |
| document  | window对象的一个属性，可以用来处理页面文档。                 |
| Navigator | 代表浏览器当前的信息，通过Navigator我们可以获取用户当前使用的是什么浏览器 |
| Location  | 代表浏览器当前的地址信息，通过Location我们可以获取或者设置当前的地址信息 |
| History   | 代表浏览器的历史信息，通过History我们可以实现上一步/刷新/下一步操作 |
| Screen    | 代表用户的屏幕信息                                           |

```javascript
//------1------
//window对象 --- 是JS的最顶层对象，其他的BOM对象都是window对象的属性。
//提供了独立于内容与浏览器窗口进行交互的对象，使用浏览器对象模型可以实现与HTML的交互。
//window.属性名 = "属性值";
window.alert('提示信息');
window.confirm("确认信息");
window.prompt("弹出输入框")
window.open("url地址"，'打开的方式（可以是-self或-black）'，'新窗口的大小'）
window.close() //关闭当前的网页。 注：存在兼容性问题
window.moveTo() //移动当前窗口(了解)注：存在兼容性问题
window.resizeTo() //调整当前窗口的尺寸
window.setTimeout(函数，时间) //只执行一次
window.setInterval(函数，时间) //无限执行
window.clearTimeout/window.clearInterval(定时器名称) //清除定时器
//Window尺寸：有三种方法能够确定浏览器窗口的尺寸。
//对于Internet Explorer、Chrome、Firefox、Opera 以及 Safari：
window.innerHeight//浏览器窗口的内部高度(包括滚动条)，浏览器可视区域的高
window.innerWidth//浏览器窗口的内部宽度(包括滚动条)，浏览器可视区域的宽
//对于 Internet Explorer 8、7、6、5：
document.documentElement.clientHeight
document.documentElement.clientWidth
//或者
document.body.clientHeight
document.body.clientWidth

//------2------
//document对象：文档对象。
//它是window对象的一个属性，可以用来处理页面文档。
document.attribute

//------3------
//location对象：浏览器当前URL信息。
//对象用于获得当前页面的地址 (URL)，并把浏览器重定向到新的页面。
window.location 对象在编写时可不使用 window 这个前缀。 一些例子：
location.herf = 'url地址';
location.hostname 返回 web 主机的域名
location.pathname 返回当前页面的路径和文件名
location.port 返回 web 主机的端口 （80 或 443）
location.portocol 返回页面使用的web协议。 http:或https:
// 重要的API：
location.reload();
location.assign();
location.replace();

//------4----
//Navigator对象：浏览器本身信息。
//window.navigator对象包含有关访问者浏览器的信息。在编写时可不使用 window 这个前缀。
navigator.platform//操作系统类型
navigator.userAgent//浏览器设定的User-Agent字符串(重要)。最常用的属性，用来完成浏览器判断
navigator.appCodeName//浏览器代号
navigator.appName//浏览器名称
navigator.appVersion//浏览器版本
navigator.language//浏览器设置的语言
navigator.systemLanguage//浏览器系统语言
navigator.cookieEnabled//浏览器是否启用了cookie

//------5------
//screen对象：客户端屏幕信息。
screen.availWidth//属性返回访问者屏幕的宽度，以像素计，减去界面特性，比如窗口任务栏。
screen.availHeight//属性返回访问者屏幕的高度，以像素计，减去界面特性，比如窗口任务栏。

//------6------
//History对象：浏览器访问历史信息。
//window.history对象包含浏览器的历史。为了保护用户隐私，对 JavaScript 访问该对象的方法做出了限制。
history.length//属性返回浏览器历史列表中的 URL 数量。
history.back()//加载历史列表中的前一个 URL。返回上一页。
history.forward()//加载历史列表中的下一个 URL。返回下一页。
history.go(-1)//负数时返回上一页，正数时返回下一页，     
```
