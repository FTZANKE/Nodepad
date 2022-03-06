# webpack中loader和plugin的区别

## 从功能作用的角度区分

1. <span style="color:red;">loader：是一个文件夹加载器，能够加载资源文件打包至指定文件中</span>

由于webpack本身只能打包commonjs规范的js文件，所以针对css，图片等格式的文件没法打包，就需要引入第三方的模块进行打包。

loader 扩展了webpack，只专注于**转化文件**这一个领域，完成压缩 / 打包 / 语言翻译等，仅仅只是为了打包，仅仅只是为了打包。

 如 css-loader 和 style-loader 模块，是为了打包css的

 如 babel-loader 和 babel-core 模块，是为了把es6的代码 转成 es5

 如 url-loader 和 file-loader，是为了把图片进行打包

2. <span style="color:red;">plugin：扩展webpack的功能，plugin是作用于webpack本身的</span>

从打包优化到压缩，到重新定义环境变量，webpack提供了很多开箱即用的插件：

 如 CommonChunkPlugin 主要用于提取第三方库和公共模块，避免首屏加载的bundle文件 或者 按需加载的bundle文件体积过大，导致加载时间过长，是优化的利器

 注：CommonsChunkPlugin于4.0及以后被移除，使用SplitChunksPlugin替代

如 html-webpack-plugin 用于html文件的拷贝，打包，还给html中自动增加了引入打包后的js文件的代码 **`<script src=""></script>`**，还能指明把js文件引入到html文件的底部等

## 从运行时机的角度区分

1. loader运行在打包文件之前，loader为在模块加载时的预处理文件

2. plugins在整个编译周期 都起作用