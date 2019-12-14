---
title: webpack4.x填坑笔记
date: 2019-06-05 20:09:37
categories:
- 打包工具
tags:
- Webpack
- 填坑
comments: false
---

## 前言
今天继续迁移一个老的react项目，因为新的脚手架起的是webpack v4.x 版本，而老项目是v1.x 版本，所以配置上有蛮多的改变...在一堆报错中冲出杀路...

### es2015报错

2017年Babel 的官网上在9月宣布 ES2015 / ES2016/ ES2017 等等 ES20xx 时代的 presets 通通被废弃（deprecated），取而代之的是 babel-preset-env，并且承诺它将成为“未来不会过时的（future-proof）”解决方案。

> 也就是说： babel preset将基于你的实际浏览器及运行环境，自动的确定babel插件及polyfills，转译ES2015及此版本以上的语言。    

解决方案：

```javascript
npm uninstall --save-dev babel-preset-es2015
npm install --save-dev babel-preset-env@next
将es2015修改成env
{
  "presets": [ "env" ],
  ...
}
```

### react报错

```javascript
Error: Cannot find module 'babel-preset-react' from '/Users/me/Desktop/reflask' - If you want to resolve "react", use "module:react" - Did you mean "@babel/react"? at Function.module.exports [as sync]
```


```javascript
webpack.config.js
 {
    test: /\.js$/,
    exclude: /node_modules/,
    use: {
        loader: "babel-loader",
        options: {
            presets: ['react']
        }
    }
},
```


解决方案：

```javascript
把
options: {
    presets: ['react']
}
but
改成
options: {
    presets: ['@babel/preset-react']
}
```

### 插件不支持
extract-text-webpack-plugin插件主要是为了抽离css样式,防止将样式打包在js中引起页面样式加载错乱的现象。但是只支持webpack3

mini-css-extract-plugin插件也是用来提取css到单独的文件的，该插件有个前提条件，只能用于webpack 4及以上的版本，所以如果使用的webpack版本低于4，,那还是用回extract-text-webpack-plugin插件。

添加依赖

```javascript
npm install --save-dev mini-css-extract-plugin
```


webpack配置

```javascript
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
module.exports = {
    plugins: [
        new MiniCssExtractPlugin({
            filename: "[name].css",
            chunkFilename: "[id].css"
        })
    ],
    module: {
        rules: [
            {
                test: /\.css$/,
              use: [
                  MiniCssExtractPlugin.loader,
                  "css-loader"
              ]
            }
        ]
    }
}
```

### 运行webpack要加参数
从4.0开始:运行webpack一定要加参数

-- mode development 或者 --mode production，

分别对应开发环境和生产环境,否则会报警告:


```javascript
"scripts": {
    "build": "webpack --optimize-minimize --mode production",
    "dev": "webpack-dev-server --config webpack.dev.config.js --mode development"
  },
```

### babel报错
现在改用babel-loader，但是换成babel-loader后也是老报错：Error: Cannot find module '@babel/core'

原因：官方默认babel-loader | babel 对应的版本需要一致: 即babel-loader需要搭配最新版本babel


```javascript
两种解决方案:

回退低版本
npm install -D babel-loader@7 babel-core babel-preset-env

更新到最高版本:
npm install -D babel-loader @babel/core @babel/preset-env webpack
```