---
title: webpack(2)
date: 2018-07-12 14:10:36
categories:
- Webpack
tags:
- 打包工具
comments: false
---

# 概念

webpack 是一个现代 JavaScript 应用程序的静态模块打包器(module bundler)。当 webpack 处理应用程序时，它会递归地构建一个依赖关系图(dependency graph)，其中包含应用程序需要的每个模块，然后将所有这些模块打包成一个或多个 bundle。

# 四个核心概念

* 入口(entry)
* 输出(output)
* loader
* 插件(plugins)

# 入口(entry)

入口起点(entry point)指示 webpack 应该使用哪个模块，来作为构建其内部依赖图的开始。进入入口起点后，webpack 会找出有哪些模块和库是入口起点（直接和间接）依赖的。每个依赖项随即被处理，最后输出到称之为 bundles 的文件中。

可以通过在 webpack 配置中配置 entry 属性，来指定一个入口起点（或多个入口起点）。默认值为 ./src。

## 单个入口（简写）语法

用法：entry: string|Array<string>

    webpack.config.js
    
    const config = {
      entry: './path/to/my/entry/file.js'
    };
    
    module.exports = config;

有失灵活性，如果需要快速为只有一个入口起点的应用程序或工具设置webpack配置的时候，是个不错的选择。

## 对象语法

用法：entry: {[entryChunkName: string]: string|

  Array<string>}

    webpack.config.js
    
    const config = {
      entry: {
        app: './src/app.js',
        vendors: './src/vendors.js'
      }
    };

## 常见场景

### 1.分离 应用程序(app) 和 第三方库(vendor) 入口

    webpack.config.js
    
    const config = {
      entry: {
        app: './src/app.js',
        vendors: './src/vendors.js'
      }
    };

### 2.多页面应用程序

    webpack.config.js
    
    const config = {
      entry: {
        pageOne: './src/pageOne/index.js',
        pageTwo: './src/pageTwo/index.js',
        pageThree: './src/pageThree/index.js'
      }
    };

根据经验：每个 HTML 文档只使用一个入口起点。

# 输出(output)

output 属性告诉 webpack 在哪里输出它所创建的 bundles，以及如何命名这些文件，默认值为 ./dist。注意，即使可以存在多个入口起点，但只指定一个输出配置。

## 用法(Usage)

在 webpack 中配置 output 属性的最低要求是，将它的值设置为一个对象，包括以下两点：

* filename 用于输出文件的文件名。
* 目标输出目录 path 的绝对路径。

      webpack.config.js
    
      const config = {
        output: {
          filename: 'bundle.js',
          path: '/home/proj/public/assets'
        }
      };
    
      module.exports = config;

## 多个入口起点

应该使用占位符(substitutions)来确保每个文件具有唯一的名称。

    {
      entry: {
        app: './src/app.js',
        search: './src/search.js'
      },
      output: {
        filename: '[name].js',
        path: __dirname + '/dist'
      }
    }
    
    // 写入到硬盘：./dist/app.js, ./dist/search.js

## 高级进阶
以下是使用 CDN 和资源 hash 的复杂示例：

    config.js
    
    output: {
      path: "/home/proj/cdn/assets/[hash]",
      publicPath: "http://cdn.example.com/assets/[hash]/"
    }
在编译时不知道最终输出文件的 publicPath 的情况下，publicPath 可以留空，并且在入口起点文件运行时动态设置。如果你在编译时不知道 publicPath，你可以先忽略它，并且在入口起点设置 __webpack_public_path__。

    __webpack_public_path__ = myRuntimePublicPath
    
    // 剩余的应用程序入口

# 模式(mode)

提供 mode 配置选项，告知 webpack 使用相应模式的内置优化。

string

## 用法
只在配置中提供 mode 选项：

    module.exports = {
      mode: 'production'
    };
或者从 CLI 参数中传递：

    webpack --mode=production
支持以下字符串值：

选项	|  描述
| - | :-: | -: |
development | 会将 process.env.NODE_ENV 的值设为 development。启用 NamedChunksPlugin 和 NamedModulesPlugin。
production | 会将 process.env.NODE_ENV 的值设为 production。启用 FlagDependencyUsagePlugin, FlagIncludedChunksPlugin, ModuleConcatenationPlugin, NoEmitOnErrorsPlugin, OccurrenceOrderPlugin, SideEffectsFlagPlugin 和 UglifyJsPlugin.
记住，只设置 NODE_ENV，则不会自动设置 mode。
## mode: development
    // webpack.development.config.js
    module.exports = {
    + mode: 'development'
    - plugins: [
    -   new webpack.NamedModulesPlugin(),
    -   new webpack.DefinePlugin({ "process.env.NODE_ENV": JSON.stringify("development") }),
    - ]
    }
## mode: production
    // webpack.production.config.js
    module.exports = {
    +  mode: 'production',
    -  plugins: [
    -    new UglifyJsPlugin(/* ... */),
    -    new webpack.DefinePlugin({ "process.env.NODE_ENV": JSON.stringify("production") }),
    -    new webpack.optimize.ModuleConcatenationPlugin(),
    -    new webpack.NoEmitOnErrorsPlugin()
    -  ]
    }

# loader

loader 让 webpack 能够去处理那些非 JavaScript 文件（webpack 自身只理解 JavaScript）。loader 可以将所有类型的文件转换为 webpack 能够处理的有效模块，然后你就可以利用 webpack 的打包能力，对它们进行处理。oader 可以将文件从不同的语言（如 TypeScript）转换为 JavaScript，或将内联图像转换为 data URL。loader 甚至允许你直接在 JavaScript 模块中 import CSS文件！

## 示例
例如，你可以使用 loader 告诉 webpack 加载 CSS 文件，或者将 TypeScript 转为 JavaScript。为此，首先安装相对应的 loader：

    npm install --save-dev css-loader
    npm install --save-dev ts-loader
然后指示 webpack 对每个 .css 使用 css-loader，以及对所有 .ts 文件使用 ts-loader：

    webpack.config.js
    
    module.exports = {
      module: {
        rules: [
          { test: /\.css$/, use: 'css-loader' },
          { test: /\.ts$/, use: 'ts-loader' }
        ]
      }
    };

## 使用 loader

* 配置（推荐）：在 webpack.config.js 文件中指定 loader。
* 内联：在每个 import 语句中显式指定 loader。
* CLI：在 shell 命令中指定它们。

## 配置[Configuration]
module.rules 允许你在 webpack 配置中指定多个 loader。 这是展示 loader 的一种简明方式，并且有助于使代码变得简洁。同时让你对各个 loader 有个全局概览：

    module: {
      rules: [
        {
          test: /\.css$/,
          use: [
            { loader: 'style-loader' },
            {
              loader: 'css-loader',
              options: {
                modules: true
              }
            }
          ]
        }
      ]
    }

# 插件(plugins)

## 用法
由于插件可以携带参数/选项，你必须在 webpack 配置中，向 plugins 属性传入 new 实例。

根据你的 webpack 用法，这里有多种方式使用插件。

## 配置
    webpack.config.js
    
    const HtmlWebpackPlugin = require('html-webpack-plugin'); //通过 npm 安装
    const webpack = require('webpack'); //访问内置的插件
    const path = require('path');
    
    const config = {
      entry: './path/to/my/entry/file.js',
      output: {
        filename: 'my-first-webpack.bundle.js',
        path: path.resolve(__dirname, 'dist')
      },
      module: {
        rules: [
          {
            test: /\.(js|jsx)$/,
            use: 'babel-loader'
          }
        ]
      },
      plugins: [
        new webpack.optimize.UglifyJsPlugin(),
        new HtmlWebpackPlugin({template: './src/index.html'})
      ]
    };
    
    module.exports = config;

# 依赖图(dependency graph)

任何时候，一个文件依赖于另一个文件，webpack 就把此视为文件之间有依赖关系。这使得 webpack 可以接收非代码资源(non-code asset)（例如图像或 web 字体），并且可以把它们作为_依赖_提供给你的应用程序。

webpack 从命令行或配置文件中定义的一个模块列表开始，处理你的应用程序。 从这些入口起点开始，webpack 递归地构建一个依赖图，这个依赖图包含着应用程序所需的每个模块，然后将所有这些模块打包为少量的 bundle - 通常只有一个 - 可由浏览器加载。