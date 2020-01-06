---
title: webpack与gulp
date: 2019-09-09 12:50:36
categories:
- 打包工具
tags:
- Webpack
- Gulp
comments: false
---



## 前言

记录 Webpack 与 Gulp 的一些知识点~

<!-- more -->



## gulp

**gulp强调的是前端开发的工作流程，我们可以通过配置一系列的task，定义task处理的事务（例如文件压缩合并、雪碧图、启动server、版本控制等），然后定义执行顺序，来让gulp执行这些task，从而构建项目的整个前端开发流程。**

**PS：简单说就一个Task Runner**

核心原理：

- 最主要是通过各种 **Transform Stream** 来实现文件的处理，然后再进行输出。Transform Stream 是 NodeJs Stream 的一种，是可读可写的，**它会对传给它的对象做一些转换的操作**
- 文件输入-》Gulp插件处理-》文件输出



## webpack

1. 一个**前端模块化**方案，更侧重**模块打包**
2. 把开发中的所有资源（图片、js文件、css文件等）都看成模块
3. 通过loader（加载器）和plugins（插件）对资源进行处理，打包成符合生产环境部署的前端资源

```js
// 入口文件  
entry: {   
    app: './src/js/index.js',  
},  
// 输出文件  
output: {   
    filename: '[name].bundle.js',   
    path: path.resolve(__dirname, 'dist'),   
    publicPath: '/'   //确保文件资源能够在 http://localhost:3000 下正确访问  
},  
// 开发者工具 source-map  
devtool: 'inline-source-map',  
// 创建开发者服务器  
devServer: {   
    contentBase: './dist',   
    hot: true        // 热更新  
},  
plugins: [   
    // 删除dist目录   
    new CleanWebpackPlugin(['dist']),   
    // 重新穿件html文件   
    new HtmlWebpackPlugin({    
        title: 'Output Management'   
    }),   
    // 以便更容易查看要修补(patch)的依赖   
    new webpack.NamedModulesPlugin(),   
    // 热更新模块   
    new webpack.HotModuleReplacementPlugin()  
],  
// 环境  
mode: "development",  
// loader配置  
module: {   
    rules: [    
        {     
            test: /\.css$/,     
            use: [      
                'style-loader',      
                'css-loader'     
            ]    
        },    
        {     
            test: /\.(png|svg|jpg|gif)$/,     
            use: [      
                'file-loader'     
            ]    
        }   
    ]  
} 
```

1、entry是页面入口文件配置，Webpack 会分析入口文件，解析包含依赖关系的各个文件，当入口文件不止一个时，可以将其配置为数组，output 是输出项配置，如例子中的输出为app.js

2、module为加载器配置，例子中定义了.css文件的加载规则，会生成style标签放在head中。webpack1的时候use中的-loader后缀可以不写，webpack2必须要写

3、plugins为插件配置，例子中定义了一个环境变量，在代码中console.log(process.env.NODE_ENV)将打印production



## 构建流程

是一个串行的过程，从启动到结束依次执行：

**初始化参数**：从配置文件和 shell 语句中读取与合并参数，得到最终的参数

**开始编译**：用得到的参数 初始化 Compiler 对象，加载所有配置的插件，执行对象的 run 方法开始执行编译

**确定入口**：根据配置中的 entry 找到所有的入口文件

**编译模块**：从入口出发，调用所有配置的 Loader 对模块进行翻译，再找出该模块依赖的模块，递归翻译，直到所有入口依赖的文件都经过了翻译

**完成模块翻译**：翻译完所有模块后，得到每个模块被翻译后的最终内容以及它们之间的依赖关系

**输出资源**：根据入口与模块之间的依赖关系，组装成一个个包含多个模块的 Chunk，再把每个 Chunk 转换成一个单独的文件加入到输出列表

**输出完成**：确定好输出内容后，根据配置确定输出的路径和文件名，把文件内容写到文件系统

在以上步骤，webpack 会在特定的时间点广播出特定的事件，插件在监听到感兴趣的事件后会执行特定的逻辑，并且插件可以调用 webpack 提供的 API 改变 webpack 的运行结果



## 什么是bundle，什么是chunk，什么是module

bundle：是由webpack打包出来的文件

chunk：是指webpack在进行模块依赖分析的时候，代码分割出来的代码块

module：是开发中的单个模块



## 什么是loader

loader用于加载某些资源文件。因为webpack本身只能打包common.js规范的js文件，对于其他资源如css，img等，是没有办法加载的，这时就需要对应的loader将资源转化，从而进行加载。



## 什么是plugin

plugin用于**扩展**webpack的功能。不同于loader，plugin的功能更加丰富，比如**压缩打包，优化，不只局限于资源的加载。**

```js
UglifyJsPlugin: 压缩代码

HotModuleReplacementPlugin 自动刷新

HtmlWebpackPlugin 依据一个简单的index.html模版，生成一个自动引用你打包后的js文件的新index.html

ExtractTextWebpackPlugin 它会将入口中引用css文件，都打包都独立的css文件中，而不是内嵌在js打包文件中

Tree-shaking 指在打包中去除那些引入了，但是在代码中没有被用到的那些死代码

在webpack中Tree-shaking是通过uglifySPlugin来Tree-shaking，Css需要使用Purify-CSS
```



**区别：**

loader 加载资源到webpack，专注转换文件这个领域

plugin 扩展webpack的功能，比如压缩代码，热加载...



## 有哪些常见的Loader？

file-loader：把文件输出到一个文件夹中，在代码中通过相对 URL 去引用输出的文件

url-loader：和 file-loader 类似，但是能在文件很小的情况下以 base64 的方式把文件内容注入到代码中去

source-map-loader：加载额外的 Source Map 文件，以方便断点调试

image-loader：加载并且压缩图片文件

babel-loader：把 ES6 转换成 ES5

css-loader：加载 CSS，支持模块化、压缩、文件导入等特性

style-loader：把 CSS 代码注入到 JavaScript 中，通过 DOM 操作去加载 CSS。

eslint-loader：通过 ESLint 检查 JavaScript 代码



## 有哪些常见的Plugin？

define-plugin：定义环境变量

commons-chunk-plugin：提取公共代码

uglifyjs-webpack-plugin：通过UglifyES压缩ES6代码



**8、webpack-dev-server和http服务器如nginx有什么区别?**

webpack-dev-server使用内存来存储webpack开发环境下的打包文件，并且可以使用模块热更新，比传统的http服务对开发更加简单高效



**9、什么 是模块热更新？**

模块热更新是webpack的一个功能，他可以使得代码修改过后不用刷新浏览器就可以更新，是高级版的自动刷新浏览器。



**10、如何自动生成webpack配置?**

webpack-cli、vue-cli



**11、什么是tree-shaking? css可以tree－shaking吗？**

指打包中去除那些引入了但在代码中没用到的死代码。在wepack中js treeshaking通过UglifyJsPlugin来进行，css中通过purify-CSS来进行.