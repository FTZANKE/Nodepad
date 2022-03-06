# raw-loader

<a herf="https://www.npmjs.com/package/raw-loader" style="color:blue;">官网在这里</a>

A loader for webpack that allows importing files as a String.[^一个用于webpack的加载器，它允许以字符串形式导入文件。]

## Getting Started[^准备开始]

To begin, you'll need to install `raw-loader`:[^首先，你需要安装 `raw-loader`]

```bash
$ npm install raw-loader --save-dev
```

Then add the loader to your `webpack` config. For example:[^然后将加载器添加到“webpack”配置中。 例如:  ]

**file.js**

```js
import txt from './file.txt';
```

**webpack.config.js**

```js
// webpack.config.js
module.exports = {
    module: {
        rules: [
            {
                test: /\.txt$/i,
                use: 'raw-loader',
            },
        ],
    },
};
```

And run `webpack` via your preferred method.[^然后通过你喜欢的方法运行“webpack”。]

## Options[^选项]

|                         Name[^名称]                          | Type[^类型] | Default[^默认] | Description[^描述]     |
| :----------------------------------------------------------: | :---------: | :------------: | :--------------------- |
| **[`esModule`](https://www.npmjs.com/package/raw-loader#esmodule)** | `{Boolean}` |     `true`     | Uses ES modules syntax |

## `esModule`

Type: `Boolean` Default: `true`

By default, `raw-loader` generates JS modules that use the ES modules syntax. There are some cases in which using ES modules is beneficial, like in the case of [module concatenation](https://webpack.js.org/plugins/module-concatenation-plugin/) and [tree shaking](https://webpack.js.org/guides/tree-shaking/).[^默认情况下，raw-loader会生成使用ES模块语法的JS模块。 在某些情况下，使用ES模块是有益的，比如module concatenation和tree shaking。  ]

You can enable a CommonJS module syntax using:[^你可以使用以下方法启用CommonJS模块语法:  ]

**webpack.config.js**

```js
module.exports = {
    module: {
        rules: [
            {
                test: /\.txt$/i,
                use: [
                    {
                        loader: 'raw-loader',
                        options: {
                            esModule: false,
                        },
                    },
                ],
            },
        ],
    },
};
```

## Examples[^例子]

### Inline[^内联]

```js
import txt from 'raw-loader!./file.txt';
```

Beware, if you already define loader(s) for extension(s) in `webpack.config.js` you should use:[^注意，如果你已经在 `webpack.config.js` 中为扩展定义了加载器，你应该使用:  ]

```js
import css from '!!raw-loader!./file.txt'; 
// Adding `!!` to a request will disable all loaders specified in the configuration
```
