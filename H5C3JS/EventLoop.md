# 1. js事件循环机制event-loop

1. 事件循环(event-loop)是什么？

总方针是先同步再异步，异步中先微任务，在宏任务。

- macro-task(宏任务)：setTimeout，setInterval
- micro-task(微任务)：Promise.then/catch，process.nextTick

# 这一次，彻底弄懂 JavaScript 执行机制

本文的目的就是要保证你彻底弄懂javascript的执行机制，如果读完本文还不懂，可以揍我。

不论你是javascript新手还是老鸟，不论是面试求职，还是日常开发工作，我们经常会遇到这样的情况：给定的几行代码，我们需要知道其输出内容和顺序。因为javascript是一门单线程语言，所以我们可以得出结论：

- javascript是按照语句出现的顺序执行的

看到这里读者要打人了：我难道不知道js是一行一行执行的？还用你说？稍安勿躁，正因为js是一行一行执行的，所以我们以为js都是这样的：

```javascript
let a = '1';
console.log(a);

let b = '2';
console.log(b);复制代码
```

![img](https://segmentfault.com/img/remote/1460000020616221)

然而实际上js是这样的：

```javascript
setTimeout(function(){
    console.log('定时器开始啦')
});

new Promise(function(resolve){
    console.log('马上执行for循环啦');
    for(var i = 0; i < 10000; i++){
        i == 99 && resolve();
    }
}).then(function(){
    console.log('执行then函数啦')
});

console.log('代码执行结束');复制代码
```

![img](https://segmentfault.com/img/remote/1460000020616222)

依照js是按照语句出现的顺序执行这个理念，我自信的写下输出结果：

```awk
//"定时器开始啦"
//"马上执行for循环啦"
//"执行then函数啦"
//"代码执行结束"复制代码
```

去chrome上验证下，结果完全不对，瞬间懵了，说好的一行一行执行的呢？

![img](https://segmentfault.com/img/remote/1460000020616223)

我们真的要彻底弄明白javascript的执行机制了。

### 1.关于javascript

javascript是一门单线程语言，在最新的HTML5中提出了Web-Worker，但javascript是单线程这一核心仍未改变。所以一切javascript版的"多线程"都是用单线程模拟出来的，一切javascript多线程都是纸老虎！

### 2.javascript事件循环

既然js是单线程，那就像只有一个窗口的银行，客户需要排队一个一个办理业务，同理js任务也要一个一个顺序执行。如果一个任务耗时过长，那么后一个任务也必须等着。那么问题来了，假如我们想浏览新闻，但是新闻包含的超清图片加载很慢，难道我们的网页要一直卡着直到图片完全显示出来？因此聪明的程序员将任务分为两类：

- 同步任务
- 异步任务

当我们打开网站时，网页的渲染过程就是一大堆同步任务，比如页面骨架和页面元素的渲染。而像加载图片音乐之类占用资源大耗时久的任务，就是异步任务。关于这部分有严格的文字定义，但本文的目的是用最小的学习成本彻底弄懂执行机制，所以我们用导图来说明：

![img](https://segmentfault.com/img/remote/1460000020616224)

导图要表达的内容用文字来表述的话：

- 同步和异步任务分别进入不同的执行"场所"，同步的进入主线程，异步的进入Event Table并注册函数。
- 当指定的事情完成时，Event Table会将这个函数移入Event Queue。
- 主线程内的任务执行完毕为空，会去Event Queue读取对应的函数，进入主线程执行。
- 上述过程会不断重复，也就是常说的Event Loop(事件循环)。

**执行流程：主线程从"任务队列"中读取执行事件，这个过程是循环不断的，这个机制被称为事件循环。此机制具体如下:主线程会不断从任务队列中按顺序取任务执行，每执行完一个任务都会检查microtask队列是否为空（执行完一个任务的具体标志是函数执行栈为空），如果不为空则会一次性执行完所有microtask。然后再进入下一个循环去任务队列中取下一个任务执行。**

我们不禁要问了，那怎么知道主线程执行栈为空啊？js引擎存在monitoring process进程，会持续不断的检查主线程执行栈是否为空，一旦为空，就会去Event Queue那里检查是否有等待被调用的函数。

说了这么多文字，不如直接一段代码更直白：

```javascript
let data = [];
$.ajax({
    url:www.javascript.com,
    data:data,
    success:() => {
        console.log('发送成功!');
    }
})
console.log('代码执行结束');复制代码
```

上面是一段简易的`ajax`请求代码：

- ajax进入Event Table，注册回调函数`success`。
- 执行`console.log('代码执行结束')`。
- ajax事件完成，回调函数`success`进入Event Queue。
- 主线程从Event Queue读取回调函数`success`并执行。

相信通过上面的文字和代码，你已经对js的执行顺序有了初步了解。接下来我们来研究进阶话题：setTimeout。

### 3.又爱又恨的setTimeout

大名鼎鼎的`setTimeout`无需再多言，大家对他的第一印象就是异步可以延时执行，我们经常这么实现延时3秒执行：

```coffeescript
setTimeout(() => {
    console.log('延时3秒');
},3000)复制代码
```

渐渐的`setTimeout`用的地方多了，问题也出现了，有时候明明写的延时3秒，实际却5，6秒才执行函数，这又咋回事啊？

先看一个例子：

```coffeescript
setTimeout(() => {
    task();
},3000)
console.log('执行console');复制代码
```

根据前面我们的结论，`setTimeout`是异步的，应该先执行`console.log`这个同步任务，所以我们的结论是：

```awk
//执行console
//task()复制代码
```

去验证一下，结果正确！
然后我们修改一下前面的代码：

```stylus
setTimeout(() => {
    task()
},3000)

sleep(10000000)复制代码
```

乍一看其实差不多嘛，但我们把这段代码在chrome执行一下，却发现控制台执行`task()`需要的时间远远超过3秒，说好的延时三秒，为啥现在需要这么长时间啊？

这时候我们需要重新理解`setTimeout`的定义。我们先说上述代码是怎么执行的：

- `task()`进入Event Table并注册,计时开始。
- 执行`sleep`函数，很慢，非常慢，计时仍在继续。
- 3秒到了，计时事件`timeout`完成，`task()`进入Event Queue，但是`sleep`也太慢了吧，还没执行完，只好等着。
- `sleep`终于执行完了，`task()`终于从Event Queue进入了主线程执行。

上述的流程走完，我们知道`setTimeout`这个函数，是经过指定时间后，把要执行的任务(本例中为`task()`)加入到Event Queue中，又因为是单线程任务要一个一个执行，如果前面的任务需要的时间太久，那么只能等着，导致真正的延迟时间远远大于3秒。

我们还经常遇到`setTimeout(fn,0)`这样的代码，0秒后执行又是什么意思呢？是不是可以立即执行呢？

答案是不会的，`setTimeout(fn,0)`的含义是，指定某个任务在主线程最早可得的空闲时间执行，意思就是不用再等多少秒了，只要主线程执行栈内的同步任务全部执行完成，栈为空就马上执行。举例说明：

```javascript
//代码1
console.log('先执行这里');
setTimeout(() => {
    console.log('执行啦')
},0);复制代码
//代码2
console.log('先执行这里');
setTimeout(() => {
    console.log('执行啦')
},3000);复制代码
```

代码1的输出结果是：

```awk
//先执行这里
//执行啦复制代码
```

代码2的输出结果是：

```awk
//先执行这里
// ... 3s later
// 执行啦复制代码
```

关于`setTimeout`要补充的是，即便主线程为空，0毫秒实际上也是达不到的。根据HTML的标准，最低是4毫秒。有兴趣的同学可以自行了解。

### 4.又恨又爱的setInterval

上面说完了`setTimeout`，当然不能错过它的孪生兄弟`setInterval`。他俩差不多，只不过后者是循环的执行。对于执行顺序来说，`setInterval`会每隔指定的时间将注册的函数置入Event Queue，如果前面的任务耗时太久，那么同样需要等待。

唯一需要注意的一点是，对于`setInterval(fn,ms)`来说，我们已经知道不是每过`ms`秒会执行一次`fn`，而是每过`ms`秒，会有`fn`进入Event Queue。一旦`setInterval`的回调函数`fn`执行时间超过了延迟时间`ms`，那么就完全看不出来有时间间隔了。这句话请读者仔细品味。

### 5.Promise与process.nextTick(callback)

传统的定时器我们已经研究过了，接着我们探究`Promise`与`process.nextTick(callback)`的表现。

`Promise`的定义和功能本文不再赘述，不了解的读者可以学习一下阮一峰老师的[Promise](https://link.segmentfault.com/?enc=EArPyRArXRKuNgw8nyIvnw%3D%3D.tBIStmj9UDNH%2Fyn2ZswWlKChYxYE%2BYymxv25LWxy3AXPX5gMweuwGecLXtGlMQpu)。而`process.nextTick(callback)`类似node.js版的"setTimeout"，在事件循环的下一次循环中调用 callback 回调函数。

我们进入正题，除了广义的同步任务和异步任务，我们对任务有更精细的定义：

- macro-task(宏任务)：包括整体代码script，setTimeout，setInterval
- micro-task(微任务)：Promise，process.nextTick

不同类型的任务会进入对应的Event Queue，比如`setTimeout`和`setInterval`会进入相同的Event Queue。

事件循环的顺序，决定js代码的执行顺序。进入整体代码(宏任务)后，开始第一次循环。接着执行所有的微任务。然后再次从宏任务开始，找到其中一个任务队列执行完毕，再执行所有的微任务。听起来有点绕，我们用文章最开始的一段代码说明：

```javascript
setTimeout(function() {
    console.log('setTimeout');
})

new Promise(function(resolve) {
    console.log('promise');
}).then(function() {
    console.log('then');
})

console.log('console');复制代码
```

- 这段代码作为宏任务，进入主线程。
- 先遇到`setTimeout`，那么将其回调函数注册后分发到宏任务Event Queue。(注册过程与上同，下文不再描述)
- 接下来遇到了`Promise`，`new Promise`立即执行，`then`函数分发到微任务Event Queue。
- 遇到`console.log()`，立即执行。
- 好啦，整体代码script作为第一个宏任务执行结束，看看有哪些微任务？我们发现了`then`在微任务Event Queue里面，执行。
- ok，第一轮事件循环结束了，我们开始第二轮循环，当然要从宏任务Event Queue开始。我们发现了宏任务Event Queue中`setTimeout`对应的回调函数，立即执行。
- 结束。

事件循环，宏任务，微任务的关系如图所示：

![img](https://segmentfault.com/img/remote/1460000020616225)

我们来分析一段较复杂的代码，看看你是否真的掌握了js的执行机制：

```javascript
console.log('1');

setTimeout(function() {
    console.log('2');
    process.nextTick(function() {
        console.log('3');
    })
    new Promise(function(resolve) {
        console.log('4');
        resolve();
    }).then(function() {
        console.log('5')
    })
})
process.nextTick(function() {
    console.log('6');
})
new Promise(function(resolve) {
    console.log('7');
    resolve();
}).then(function() {
    console.log('8')
})

setTimeout(function() {
    console.log('9');
    process.nextTick(function() {
        console.log('10');
    })
    new Promise(function(resolve) {
        console.log('11');
        resolve();
    }).then(function() {
        console.log('12')
    })
})复制代码
```

第一轮事件循环流程分析如下：

- 整体script作为第一个宏任务进入主线程，遇到`console.log`，输出1。
- 遇到`setTimeout`，其回调函数被分发到宏任务Event Queue中。我们暂且记为`setTimeout1`。
- 遇到`process.nextTick()`，其回调函数被分发到微任务Event Queue中。我们记为`process1`。
- 遇到`Promise`，`new Promise`直接执行，输出7。`then`被分发到微任务Event Queue中。我们记为`then1`。
- 又遇到了`setTimeout`，其回调函数被分发到宏任务Event Queue中，我们记为`setTimeout2`。

宏任务Event Queue

微任务Event Queue

setTimeout1

process1

setTimeout2

then1

- 上表是第一轮事件循环宏任务结束时各Event Queue的情况，此时已经输出了1和7。
- 我们发现了`process1`和`then1`两个微任务。
- 执行`process1`,输出6。
- 执行`then1`，输出8。

好了，第一轮事件循环正式结束，这一轮的结果是输出1，7，6，8。那么第二轮时间循环从`setTimeout1`宏任务开始：

- 首先输出2。接下来遇到了`process.nextTick()`，同样将其分发到微任务Event Queue中，记为`process2`。`new Promise`立即执行输出4，`then`也分发到微任务Event Queue中，记为`then2`。

宏任务Event Queue

微任务Event Queue

setTimeout2

process2

 

then2

- 第二轮事件循环宏任务结束，我们发现有`process2`和`then2`两个微任务可以执行。
- 输出3。
- 输出5。
- 第二轮事件循环结束，第二轮输出2，4，3，5。
- 第三轮事件循环开始，此时只剩setTimeout2了，执行。
- 直接输出9。
- 将`process.nextTick()`分发到微任务Event Queue中。记为`process3`。
- 直接执行`new Promise`，输出11。
- 将`then`分发到微任务Event Queue中，记为`then3`。

宏任务Event Queue

微任务Event Queue

 

process3

 

then3

- 第三轮事件循环宏任务执行结束，执行两个微任务`process3`和`then3`。
- 输出10。
- 输出12。
- 第三轮事件循环结束，第三轮输出9，11，10，12。

整段代码，共进行了三次事件循环，完整的输出为1，7，6，8，2，4，3，5，9，11，10，12。
(请注意，node环境下的事件监听依赖libuv与前端环境不完全相同，输出顺序可能会有误差)

### 6.写在最后

#### (1)js的异步

我们从最开头就说javascript是一门单线程语言，不管是什么新框架新语法糖实现的所谓异步，其实都是用同步的方法去模拟的，牢牢把握住单线程这点非常重要。

#### (2)事件循环Event Loop

事件循环是js实现异步的一种方法，也是js的执行机制。

#### (3)javascript的执行和运行

执行和运行有很大的区别，javascript在不同的环境下，比如node，浏览器，Ringo等等，执行方式是不同的。而运行大多指javascript解析引擎，是统一的。

#### (4)setImmediate

微任务和宏任务还有很多种类，比如`setImmediate`等等，执行都是有共同点的，有兴趣的同学可以自行了解。

#### (5)最后的最后

- javascript是一门单线程语言
- Event Loop是javascript的执行机制

牢牢把握两个基本点，以认真学习javascript为中心，早日实现成为前端高手的伟大梦想！



---------------------

# 2. JavaScript 事件循环机制 - 微任务和宏任务的关系

# 事件循环机制，微任务和宏任务的关系

> 本文涉及到的名词：事件循环（Event Loop），宏任务（macro-task）与微任务（micro-task），执行栈和任务队列等。

## 前言

JavaScript 是单线程的，同一时间只能做一件事情。如果碰到某个耗时长的任务（比如一个需要 3s 的网络请求），那么后续的任务都要等待，这种效果是无法接受的，这时我们就引入了[异步任务](https://www.wenyuanblog.com/blogs/javascript-event-loop.html)的概念。

所以 JavaScript 执行主要包括同步任务和异步任务：  

**同步任务**：会放入到执行栈中，他们是要按顺序执行的任务；  

**异步任务**：会放入到任务队列中，这些异步任务一定要等到执行栈清空后才会执行，也就是说异步任务一定是在同步任务之后执行的。

本文所讲的 JavaScript 事件循环机制，它主要与异步任务有关。

## 任务队列

事件循环主要与任务队列有关，所以必须要先知道宏任务与微任务。

在任务队列中，有两种任务：宏任务和微任务。

**宏任务**：script 标签中的整体代码、setTimeout、setInterval、setImmediate、I/0、UI渲染  

**微任务**：process.nextTick（Node.js）、Promise、Object.observe（不常用）、MutationObserver（Node.js）

**任务优先级**：process.nextTick > Promise.then > setTimeout > setImmediate

以上这些是常见的宏任务和微任务，记住就行了，不用追究为什么它是宏任务或微任务，因为就是这样的。

## 事件循环

那么什么是事件循环机制呢？

- 一开始整个脚本（script 标签中的整体代码）作为一个宏任务执行；
- 执行过程中同步代码直接执行，宏任务进入宏任务队列，微任务进入微任务队列；
- 当前宏任务执行完毕后，立即执行当前微任务队列中的所有微任务（依次执行）；
- 当前宏任务执行完毕，开始检查渲染，然后 GUI 线程接管渲染（浏览器会在两个宏任务交接期间，对页面进行重新渲染）；
- 渲染完毕后，JavaScript 线程继续接管，开始下一个宏任务（从任务队列中获取），依此循环，直到宏任务和微任务队列都为空。

上面这一过程就称为：事件循环（Event Loop）。

说的通俗一点：微任务是小跟班，一直跟在**当前**宏任务后面：代码执行过程中，每当碰到一个微任务，就马上跟在当前宏任务后面；当碰到一个宏任务，那不好意思你排到下一次循环再说。

## 代码示例

我们执行如下一段代码，用上面的思路执行，看一下结果是否和预期的一致。

```js
console.log('script start')

// 宏任务
setTimeout(() => {
  console.log('setTimeout')
}, 0)

// 微任务 跟在当前宏任务后面
new Promise((resolve) => {
  console.log('new Promise')
  resolve()
  console.log('promise body')
}).then(() => {
  console.log('promise.then 1')
}).then(() => {
  console.log('promise.then 2')
})

console.log('script end')
```

按照上面的思路，我们来理一下，预测一下执行结果，看看实际效果是否是这样的。

执行流程：

- 第一次事件循环
  - 首先这一整段 JavaScript 代码作为一个宏任务先被执行
  - 遇到 `console.log('script start')`，打印出 `"script start"`；
  - 遇到 `setTimeout`，回调函数作为宏任务压入到宏任务队列中，此时宏任务队列：`[setTimeout]`；
  - 遇到 `new Promise`，由于 `new` 一个对象是瞬间执行的，不是异步，所以打印出 `"new Promise"`；
  - 继续执行，由于 Promise 中的异步逻辑在 `then` 里面，在 `then` 之前的都不是异步，所以打印出 `"promise body"`；
  - 遇到了第一个 `.then`，它是个微任务，将它放入微任务队列，跟在当前宏任务（整体代码）后面，此时微任务队列：`[promise 1]`；
  - Promise 的第一个 `.then` 还没执行，只是排好队伍了，因此继续往后，遇到 `console.log('script end')`，打印出 `"script end"`。
  - 执行第一个宏任务后的微任务
  - 执行 Promise 的第一个 `.then`，打印出 `"promise 1"`，，此时微任务队列：`[]`；
  - 又遇到 `.then`，它是个微任务，将它放入微任务队列，跟在当前宏任务（整体代码）后面，此时微任务队列：`[promise 2]`；
  - 执行 Promise 的第二个 `.then`，打印出 `"promise 2"`，此时微任务队列：`[]`；
  - 整体代码执行完，微任务队列也执行完，当前的事件循环结束。
- 第二次事件循环
  - 执行 `setTimeout` 的回调，打印出 `"setTimeout"`。

预测打印结果：

```js
"script start"
"new Promise"
"promise body"
"script end"
"promise.then 1"
"promise.then 2"
"setTimeout"
```

执行代码后可以发现，实际打印结果和预测一致。

## 复杂情况

如果遇到更复杂的场景，比如当前微任务里有微任务，微任务里有宏任务，多层嵌套的情况，只需记住一句话：**微任务跟在当前宏任务后面，执行完当前宏任务，微任务就跟上，然后再执行下一个宏任务**。

## 应用场景

除了在前端面试中，会问到关于事件循环、执行栈的问题，了解 JavaScript 事件循环机制有没有实质的作用呢？

- 以后我们在代码中使用 Promise，setTimeout 时，思路将更加清晰，用起来更佳得心应手；
- 在阅读一些源码时，对于一些 setTimeout 相关的骚操作可以理解的更加深入；
- 理解 JavaScript 中的任务执行流程，加深对异步流程的理解，少犯错误。

## 总结

- JavaScript 事件循环总是从一个宏任务开始执行；
- 一个事件循环过程中，只执行一个宏任务，但是可能执行多个微任务；
- 执行栈中的任务产生的微任务会在当前事件循环内执行；
- 执行栈中的任务产生的宏任务要在下一次事件循环才会执行。

最后的最后，记住，JavaScript 是一门单线程语言，异步操作都是放到事件循环队列里面，等待主执行栈来执行的，并没有专门的异步执行线程。







----

# 3. 下面代码的执行顺序

```javascript
console.log(1);

settimeout(() => {
    console.log(2);
}, 0);

new Promise((resolve, reject) => {
    console.log(3);
    resolve();
}).then(() => {
    console.log(4)
});

console.log(5);

// 1 => 3 => 5 => 4 => 2
```

1. JavaScript 的所有代码都在主线程执行，形成一个执行栈.
2. 主线程以外，还存在着一个任务队列，只要有了异步代码，就在任务队列放置一个事件.
3. 一旦执行栈所有的同步任务执行完毕以后，系统就会去读取任务队列，对应的异步任务就会结束等待的状态，进入执行栈，开始执行
4. 最先执行的是同步任务，执行完毕后，立即出栈，让出主线程。然后开始执行任务队列中的异步任务
5. 任务队列：存在着两个队列，一个是宏任务队列，一个是微任务队列（ 异步任务分为宏任务和微任务 ）
6. JavaScript 代码的执行顺序： 同步任务 -> 微任务 -> 宏任务
7. Promise 本身是同步任务，它的then catch finally 是异步任务。
8. 定时器属于宏任务