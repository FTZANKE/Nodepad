## call,apply,bind区别

三者都可以传参，但是apply是数组，而call是参数列表，且apply和call是一次性传入参数，而bind可以分为多次传入。 bind 是返回绑定this之后的函数，便于稍后调用；apply 、call 则是立即执行 。