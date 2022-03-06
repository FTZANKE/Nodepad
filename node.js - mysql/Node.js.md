# node.js

## require()===>用来加载模块

## 引入模块的方法

使用这些模块，最常见的方法是调用“require('name')”并将返回值赋给一个和模块同名的局部变量。 例如：

```js 
var http = require('http');
```

## 简单创建 web 服务器

```js
// 引入http模块
var http = require('http');
// 创建服务
http.createServer(function (request, response) {
    response.writeHead(200, { 'Content-Type': 'text/plain' });
    response.end('success');
}).listen(8001);
// request(向服务器请求的数据)==>请求
// response(向客户端响应的数据)==>响应
// response.end('text')==>响应后返回的text，可自定义
// listen(8001)==>服务绑定到本地8001端口
console.log('Server running at http://localhost:8001/');
```

## http.Server

```js
// 使此服务器停止接受任何新连接。
server.close()
// 设置此属性使服务器的连接数高于此数值时拒绝连接。
server.maxConnections
// 代表当前服务器的连接数
server.connections
```

## Path模块

- **path.join([path1], [path2], [...])**

```js
node> require('path').join('/foo', 'bar', 'baz/asdf', 'quux', '..')
// '/foo/bar/baz/asdf'
```

- **path.normalize(p)**

  转化路径字符串，将'..'和'.'替换为实际的路径

```js
path.normalize('/foo/bar/baz/asdf/quux/..')
// '/foo/bar/baz/asdf'
```

- **path.dirname(p)**

  返回路径中代表文件夹的部分

```js
path.dirname('/foo/bar/baz/asdf/quux')
// '/foo/bar/baz/asdf'
```

- **path.basename(p, [ext])**

  返回路径中的最后一部分

```js
path.basename('/foo/bar/baz/asdf/quux.html')
// 'quux.html'
path.basename('/foo/bar/baz/asdf/quux.html', '.html')
// 'quux'
```

- **path.extname(p)**

  返回路径中文件的后缀名，即路径中最后一个'.'之后的部分。如果一个路径中并不包含'.'或该路径只包含一个'.' 且这个'.'为路径的第一个字符，则此命令返回空字符串

```js
path.extname('index.html')
// '.html'
path.extname('index')
```

- **path.exists(p, [callback])**

  检测给定的文件路径是否存在。然后传递结果(true 或false)给回调函数

```js
path.exists('/etc/passwd', function (exists) {
    sys.debug(exists ? "it's there" : "no passwd!");
});
```

# npm

## 1. 介绍

npm包有很多的镜像源，有的源有的时候访问失败，有的源可能没有最新的包等等，所以有时需要切换npm的源，nrm包就是解决快速切换问题的。
nrm可以帮助您在不同的npm源地址之间轻松快速地切换。

nrm内置了如下源：

| 源        | URL                                                          | 主页                                                         |
| --------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| npm       | [https://registry.npmjs.org/](https://link.segmentfault.com/?enc=31baqybs8BuPdGxgeu%2FEkA%3D%3D.uj6O7qSHFVZbSmqJcrNBhJz1A6hSExo2vdah9aRcS3A%3D) | [https://www.npmjs.com/](https://link.segmentfault.com/?enc=m0M40t0IyQFbf5WH2HboYg%3D%3D.J1sATqL%2B4WsYmtOAuWHTtzj8yhid%2B0YHlfEJLHi2fAQ%3D) |
| cnpm      | [http://r.cnpmjs.org/](https://link.segmentfault.com/?enc=NMPXo%2FSBerat%2FEFfkXxGJg%3D%3D.KqFWB4g9%2FbPTyeyV4dg5KTPMF6VoZhI0XBRN4tfRoY8%3D) | [https://cnpmjs.org/](https://link.segmentfault.com/?enc=TjE6JTG3t0TXpbXcOJSd%2FQ%3D%3D.e8PSwWTgOL%2FD6Q0oTSYwOKwFxf10qYBkx2kKR7DUV88%3D) |
| taobao    | [https://registry.npm.taobao.org/](https://link.segmentfault.com/?enc=lx3C%2BvKvcR9VUQJpA52HsA%3D%3D.gSdtgXM%2FSkwl11gZXdT2O4XjbeROSUk9uKjZAowuUhV5fgjYvEe5DJWdDwpzLttT) | [https://npm.taobao.org/](https://link.segmentfault.com/?enc=dUxEWgO5o4a2NHo3diWx8g%3D%3D.k9OnQPxbUacaXKQTGCT4j5AiAsJTvaapXGdgldtZ0t8%3D) |
| npmMirror | [https://skimdb.npmjs.com/regi...](https://link.segmentfault.com/?enc=y5OuXQmmL8cPkWo%2BgtYoOg%3D%3D.vkl0Mm2zWdPfZIrC1VjhfFMYIhRYp2jv%2BIERPSByPyaZ7ayHtgG9y05fuzCfUIPj) | [https://skimdb.npmjs.com/](https://link.segmentfault.com/?enc=bOg3qofH5XdixHOFzXhBhw%3D%3D.y%2F9KRf7J85UXJ%2BAR7NXA6vmpeBkdK32SW5IXQeKnMho%3D) |
| nj        | [https://registry.nodejitsu.com/](https://link.segmentfault.com/?enc=ldRi1RfDidqXiXLnruhkcg%3D%3D.T2cR4gWeOwrVsUZOm49l%2B%2F8RRbzTE5F%2BzfpnP47Rcg8%3D) | [https://www.nodejitsu.com/](https://link.segmentfault.com/?enc=oeEuH8GIzkvCDG5EV%2Bh2AQ%3D%3D.Nw%2BgwhnJjE5didRIqsf67t%2Fm6O5fzPKwlHPImtE0jjI%3D) |
| rednpm    | [http://registry.mirror.cqupt....](https://link.segmentfault.com/?enc=zHs5M6fJCSMtiVeZ4tBM9A%3D%3D.RVRYTVYY0EXzA1HTIURGjejiy6FMj%2BYk%2FDicQllDnapunKjYroP8%2Fn%2FS2bTOzIqJ) | [http://npm.mirror.cqupt.edu.cn/](https://link.segmentfault.com/?enc=idwX4ciXac5gSouMgSNJdA%3D%3D.XUXETt%2BrzUr0gTSpYdBXL5LXyxJ4jvIM%2BXeT%2FXrMa8U%3D) |
| edunpm    | [http://registry.enpmjs.org/](https://link.segmentfault.com/?enc=PDXgWxKIMQYIlHf8x%2Byb1Q%3D%3D.fOzqT6t9EJki7SUYQweiEwIpqiAn91CsHEJLl9xfjMk%3D) | [http://www.enpmjs.org/](https://link.segmentfault.com/?enc=0YPWpDb3OayfLjgYoj%2BKww%3D%3D.LcyM0v573JXXxe3KXtiQ9oJaUJABG9cuilOi3lCo%2BII%3D) |

经过本人实测，nj、rednpm、edunpm 源访问失败(2018-12-19)。

## 2. 安装

## 打开终端运行`npm install -g nrm`命令：

```gradle
~ npm install -g nrm

/usr/local/bin/nrm -> /usr/local/lib/node_modules/nrm/cli.js
+ nrm@1.0.2
added 324 packages from 564 contributors in 13.338s
```

![clipboard.png](https://segmentfault.com/img/bVblfTK?w=1378&h=350)

查看是否安装成功:`nrm --version`

```ada
~ nrm --version

1.0.2
```

![clipboard.png](https://segmentfault.com/img/bVblfT5?w=830&h=112)

## 3. 使用

### 3.1 列出可选择的源:nrm ls

```
nrm ls
~ nrm ls

  npm ---- https://registry.npmjs.org/
  cnpm --- http://r.cnpmjs.org/
* taobao - https://registry.npm.taobao.org/
  nj ----- https://registry.nodejitsu.com/
  rednpm - http://registry.mirror.cqupt.edu.cn/
  npmMirror  https://skimdb.npmjs.com/registry/
  edunpm - http://registry.enpmjs.org/
```

***注：\*** 前面带 * 号的表示正在使用的源

![clipboard.png](https://segmentfault.com/img/bVblfUU?w=1108&h=486)

### 3.2 切换使用的源

```
nrm use npm
~ nrm use npm
                        
   Registry has been set to: https://registry.npmjs.org/
```

![clipboard.png](https://segmentfault.com/img/bVblfVi?w=1272&h=444)

### 3.3 添加一个源:nrm add <registry> <url>

如果你想添加一个源，终端执行命令`nrm add <registry> <url> [home]`，reigstry为源名，url为源的路径， home为源的主页(可不写)。

```ruby
 ~ nrm add company http://npm.company.com/   

    add registry company success
```

![clipboard.png](https://segmentfault.com/img/bVblfYr?w=1282&h=1144)

***注：\***

1. URL最后的/也可以不带，下面两个URL都是可以的：
   `http://npm.company.com/`
   `http://npm.company.com`
2. [home]参数用于`nrm home`命令，用来查看源的主页。

### 3.4 删除一个源

想要删除一个源，终端执行命令`nrm del <registry>`，reigstry为源名.

```maxima
 ~ nrm del company

    delete registry company success
```

![clipboard.png](https://segmentfault.com/img/bVblf3u?w=1224&h=1148)

***注：\***nrm del 命令不能删除 nrm 自己内置的源。

### 3.5 测试源速度

测试一个源的响应时间：`nrm test npm`

```coffeescript
~ nrm test npm

* npm ---- 833ms
```

测试所有源的速度：`nrm test`

```routeros
~ nrm test

* npm ---- 807ms
  cnpm --- 374ms
  taobao - 209ms
  nj ----- Fetch Error
  rednpm - Fetch Error
  npmMirror  1056ms
  edunpm - Fetch Error
```

### 3.6 访问源的主页

如果你想访问源的主页，可在终端输入下面命令：

```arduino
nrm home taobao
```

此命令会在浏览器中打开淘宝源的主页：[https://npm.taobao.org/](https://link.segmentfault.com/?enc=B0QztpIRqT18e5AwzCTSGA%3D%3D.%2Fy21dPx3Zv0Dxs2JrDd3ETXg4JQggNXlxyLOX160baM%3D)

***注：\***
如果要查看自己添加的源的主页，那么在添加源的时候就要把主页带上：

```awk
~ nrm add company http://npm.company.com/ http://npm.company.com/
```

**如果添加源的时候没有写home信息，那么`nrm home`命令不会有效果。**

## 4. 不使用nrm来切换源

如果不是nrm也能切换源，只不过比较麻烦。

- 查看当前使用的源
  `npm config get registry`

  ```arduino
  ~ npm config get registry
  https://registry.npmjs.org/
  ```

- 设置一个源

  `npm config set registry https://registry.npm.taobao.org/`

  ```arduino
  ~ npm config set registry https://registry.npm.taobao.org/
  ```

  设置成功后终端不会有任何输出。
  ![clipboard.png](https://segmentfault.com/img/bVblf9h?w=1416&h=278)

- 安装包使用特定源
  全部使用特定源安装：`npm install --registry=https://registry.npm.taobao.org`
  安装一个包使用特定源：`npm i logo --registry=https://registry.npm.taobao.org`