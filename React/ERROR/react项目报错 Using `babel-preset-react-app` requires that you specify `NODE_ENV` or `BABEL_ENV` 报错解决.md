react项目报错 Using `babel-preset-react-app` requires that you specify `NODE_ENV` or `BABEL_ENV` 报错解决

解决方法：

在`package.json`文件`eslintConfig`中添加如下。

```json
"parserOptions": {
    "babelOptions": {
        "presets": [
            ["babel-preset-react-app", false],
            ["babel-preset-react-app/prod"]
        ]
    }
}
```

该问题的具体描述信息
https://github.com/facebook/create-react-app/issues/12070
————————————————
版权声明：本文为CSDN博主「TangAcrab」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/weixin_39786582/article/details/125068541