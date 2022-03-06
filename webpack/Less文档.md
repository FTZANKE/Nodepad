# Less 上手文档

## node.js 安装

```ruby
npm install -g less
```
## 编译less

```ruby
lessc styles.less styles.css
```

## 引入方式

```html
<!-- 正确的 less 文件引入，依赖 less.js(less在前，js在后) -->
<link rel="stylesheet/less" type="text/css" href="styles.less" />
<script src="https://cdn.bootcdn.net/ajax/libs/less.js/4.1.1/less.js"></script>
<!-- 直接引入编译后的 css 文件也可以 -->
<link rel="stylesheet" href="styles.css" />
```



## 定义变量

```less
@height: 40px; // 定义变量名的规范：@name,分号必须带

.box{
    height: @height; // 这样子应用
}
```

## 嵌套语法

```html
<div class="box">
    <a href="#">Less 如此强大</a>
    <ul class="list">
        <li>上手简单</li>
        <li>语法丰富</li>
    </ul>
    <ul class="list2">
        <li>上手简单</li>
        <li>语法丰富</li>
    </ul>
</div>
```



```less
@height: 40px;

* {
    margin: 0;
    padding: 0;
}

.box {
    a {
        color: black;
    }

    ul,
    ol,
    li {
        list-style: none;
    }

    .list {
        li {
            height: @height;
            line-height: @height;
            font-size: 16px;
            color: aqua;
        }
    }

    .list1 {
        li {
            height: @height;
            line-height: @height;
            font-size: 16px;
            color: blueviolet;
        }
    }
}
```

## 运算语法

```less
// 所有操作数被转换成相同的单位
@conversion-1: 5cm + 10mm; // 结果是 6cm
@conversion-2: 2 - 3cm - 5mm; // 结果是 -1.5cm

@incompatible-units: 2 + 5px - 3cm; // 结果是 4px

@base: 5%;
@filler: @base * 2; // 结果是 10%
@other: @base + @filler; // 结果是 15%

@base: 2cm * 3mm; // 结果是 6cm

@color: #224488 / 2; //结果是 #112244
background-color: #112244 + #111; // 结果是 #223355
```

## 转义

```less
@min768: ~"(min-width: 768px)";
.element {
    @media @min768 {
        font-size: 1.2rem;
    }
}
// ==> 编译后
@media (min-width: 768px) {
    .element {
        font-size: 1.2rem;
    }
}
```

简写：

```less
@min768: (min-width: 768px);
.element {
    @media @min768 {
        font-size: 1.2rem;
    }
}
```

## 函数语法

```less
@base: #f04615;
@width: 0.5;

.class {
    width: percentage(@width); // returns `50%`
    color: saturate(@base, 5%);
    background-color: spin(lighten(@base, 25%), 8);
}
```

## 映射

从 Less 3.5 版本开始，还可以将混合（mixins）和规则集（rulesets）作为一组值的映射（map）使用。

```less
#colors() {
    primary: blue;
    secondary: green;
}

.button {
    color: #colors[primary];
    border: 1px solid #colors[secondary];
}
```

输出符合预期：

```css
.button {
    color: blue;
    border: 1px solid green;
}
```

## 命名空间和访问符

假设你希望将一些混合（mixins）和变量置于 `#bundle` 之下，为了以后方便重用或分发：

```less
#bundle() {
    .button {
        display: block;
        border: 1px solid black;
        background-color: grey;
        &:hover {
            background-color: white;
        }
    }
    .tab { ... }
    .citation { ... }
}
```

现在，如果我们希望把 `.button` 类混合到 `#header a` 中，我们可以这样做：

```less
#header a {
    color: orange;
    #bundle.button();  // 还可以书写为 #bundle > .button 形式
}
```

注意：如果不希望它们出现在输出的 CSS 中，例如 `#bundle .tab`，请将 `()` 附加到命名空间（例如 `#bundle()`）后面。

## 注释

块注释和行注释都可以使用：

```less
/* 一个块注释
* style comment! */
@var: red;

// 这一行被注释掉了！
@var: white;
```

## 作用域

Less 中的作用域与 CSS 中的作用域非常类似。首先在本地查找变量和混合（mixins），如果找不到，则从“父”级作用域继承。

```less
@var: red;

#page {
    @var: white;
    #header {
        color: @var; // white
    }
}
```

## 导入

“导入”的工作方式和你预期的一样。你可以导入一个 `.less` 文件，此文件中的所有变量就可以全部使用了。如果导入的文件是 `.less` 扩展名，则可以将扩展名省略掉：

```css
@import "library"; // library.less
@import "typo.css";
```
