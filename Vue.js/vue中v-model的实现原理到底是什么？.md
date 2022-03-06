# vue中v-model的实现原理到底是什么？

v-model是语法糖。
v-model原理：

1. v-bind绑定响应式数据
2. 通过oninput触发事件获取当前$event.target.value，然后赋值给当前变量。
3. 当前变量值修改，会触发object.definedProperty中的set方法，将data中的当前变量进行赋值。