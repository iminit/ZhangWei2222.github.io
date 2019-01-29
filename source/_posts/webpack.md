---
title: webpack
date: 2017-08-18 15:15:47
categories:
- 前端技术篇
tags:
- 打包工具
- webpack
comments: false
---

### 一、Hello Webpack
#### 1.webpack介绍
webpack是一个现代的JavaScript应用模块打包器（module bundler），它能把各种资源，例如JS（含JSX）、coffee、样式（less/sass）、图片等都作为模块来处理和使用。它有着难以置信的配置和神奇的效果，大大提高了我们程序员的工作效率。举个例子：我们可以直接使用require（xxx）的形式来引入各模块，即使他们可能需要经过编译（比如JSX和sass），但我们无须再上面花费太多心思，webpack有着各种健全的加载器（loader）在默默处理这些事情。它可以部分代替Grunt和gulp的功能。

#### 2.webpack的安装
1、全局安装


```
$npm install webpack -g
```

2、直接安装到项目的依赖里，也就是写入package.json


```
npm init
npm install webpack –save-dev
```


#### 3.配置文件webpack.config.js
重点:什么是入口文件？模块打包的起点称之为入口起点（entry point）。入口起点告诉webpack从哪里开始，并遵循着依赖关系进行打包。可以将您的应用入口起点认为是根上下文（contextual root）或app第一个启动文件。

#### 4.我们来作一个实例（Hello World！）
1.建立index.html文件

2.建立main.js文件

3.建立webpack.config.js文件

4.命令行输入webpack

5.安装live-server 并预览程序

index.html


```
<!DOCTYPE html>
<html lang="en">

<head>
    <title></title>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">

</head>

<body>
    <script type="text/javascript" src="bundle.js"></script>
</body>

</html>
```

main.js

```
document.write("<h1>Hello World</h1>");
webpack.config.js

module.exports = {
    entry: "./main.js",
    output: {
        filename: 'bundle.js'
    }
};
```

安装live-server,只要用npm install就可以安装了。


```
npm install live-server -g
```

### 二、多入口文件（Multiple entry files）
#### 1.webpack配置多路口文件
当我们需要时webpack 是允许我们有多个输出文件的。也就是说，我们可以在html文件中引入2个js文件或者其他的文件。

我们来做个多入口文件的例子：

1、首先我们建立两个JavaScript文件，分别是main1.js 和main2.js。然后在里边输出 Hello world 和 Hello webpack 。

main1.js


```
document.write("<h1>Hello World!</h1>");
```

main2.js


```
document.write("<h2>Hello Webpack!</h2>");
```

#### 2、建立indx.html文件，引入bundle1.js和bundle2.js，这两个文件就是我们要打包的两个js文件。

index.html


```
<!DOCTYPE html>
<html lang="en">

<head>
    <title>webpack example02</title>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">

</head>

<body>
    <script type="text/javascript" src="./bundle1.js"></script>
    <script type="text/javascript" src="./bundle2.js"></script>
</body>

</html>
```

#### 3、建立webpack.config.js配置文件。


```
module.exports = {
    entry: {
        bundle1: './main1.js',
        bundle2: './main2.js'
    },
    output: {
        filename: '[name].js'
    }
};
```

### 三、使用Webpack CSS loader加载器
#### 1.什么是loader
loader用于转换应用程序的资源文件，他们是运行在nodejs下的函数，使用参数来获取一个资源的来源并且返回一个新的来源（资源的位置），例如：你可以使用loader来告诉webpack去加载一个coffeeScript或者Babel文件。

#### 2.loader的解析
loader的解析类似模块，一个loader模块会导出一个方法并且可被nodejs写为可兼容的JavaScript,通常情况下通过npm来管理loader,但你也可以把loader放在自己的应用里。

#### 3.安装loader
你可以通过npm来安装loader


```
$ npm install xxx-loader --save-dev
```

或者


```
$ npm install xxx-loader --save
```

#### 4.CSS-loader 实例
1、先建立一个html文件，并引入bundle.js文件。

index.html


```
<!DOCTYPE html>
<html lang="en">

<head>
    <title></title>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">

</head>

<body>
    <script type="text/javascript" src="./bundle.js"></script>
</body>

</html>
```

2、建立main.js文件，并用require引入app.css文件。然后写一句话。

main.js


```
require('./app.css');
document.write('<h1>Hello Webpack!</h1>');
```

3、建立app.css文件，把body 的背景设置为红色。

app.css


```
body {
    background-color: red;
}
```

4、建立webpack.config.js配置文件，在里边加入加载器。


```
module.exports = {
    entry: './main.js',
    output: {
        filename: 'bundle.js'
    },
    module: {
        loaders: [
            { test: /\.css$/, loader: 'style-loader!css-loader!stylus-loader' },
        ]
    }
};
```

5、用npm 安装 css-loader 和style-loader


```
$ npm install style-loader --save-dev

$ npm install css-loader --save-dev

npm install stylus-loader stylus --save-dev
```

6、在控制台输入 webpack 进行打包


```
$ webpack
```

这样就用webpack的加载器成功打包了css样式文件。以后我们的html里就可以不用看到css的引入语句了。

### 四、使用webpack Image loader 加载图片
直接用一个例子说明
#### 1、新建main.js文件， 创建img标签后，把src的值用require引进来。然后插入标签。


```
var img1 = document.createElement("img");
img1.src = require("./small.png");
document.body.appendChild(img1);

var img2 = document.createElement("img");
img2.src = require("./big.png");
document.body.appendChild(img2);
```

#### 2、建立index.html文件，引入bundle.js

<!DOCTYPE html>

```
<html lang="en">

<head>
    <title></title>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">

</head>

<body>
    <script type="text/javascript" src="bundle.js"></script>
</body>

</html>
```

#### 3、建立webpack.config.js配置文件


```
module.exports = {
    entry: './main.js',
    output: {
        filename: 'bundle.js'
    },
    module: {
        loaders: [
            { test: /\.(png|jpg)$/, loader: 'url-loader?limit=8192' },
        ]
    }
};
```

这里需要说明的是limit ，它的左右是如果图片的大小，小于8192bytes就以Data URL的形式引入图片，大于就用图片地址引用。

#### 4、打开命令行，用cnpm 安装url-loader包。


```
$ cnpm install url-loader --save-dev
```

#### 5、用webpack命令进行打包


```
$ webpack
```

### 五、使用uglify-js 压缩打包JS代码
#### 1.webpack 插件机制介绍：
插件可以完成更多loader不能完成的功能。插件的使用一般是在webpack的配置信息 plugins选定中指定。Webpack本身内置了一些常用的插件，还可以通过npm安装第三方插件。

#### 2.uglify-js介绍：
ugligy-js是一个用npm安装的JavaScript代码压工具，我们在grunt和gulp中经常使用。

用npm命令进行安装：


```
npm install uglify-js g
```

最常用的方法：


```
uglifyjs [input files] [options]

uglifyjs demo.js -o demo-min.js -c
```

#### 3.用Webpack中的uglify-js压缩Demo
这里我就不贴出index.html和main.js的代码了，我只贴出webpack.config.js的代码。如果你想完整的做出例子，可以按照视频步骤一步一步的操作。


```
var webpack = require('webpack');
module.exports = {
    entry: './main.js',
    output: {
        path: __dirname,
        filename: 'bundle.js'
    },
    plugins: [
        new webpack.optimize.UglifyJsPlugin({
            compress: {
                warnings: false
            }
        })
    ]
};
```

### 六、使用webpack构建本地服务器
我们之前一直都在用live-server充当本地服务器，其实它和webpack是不搭的，我们可以利用webpack自带的webpack-dev-server来构建一个本地服务器。它让你的浏览器检测你的代码修改，并自动刷新修改后的结果，它是基于node.js构建。webpack-dev-server是一个单独的组件，在webpack中进行配置之前需要单独安装它作为项目依赖。

#### 1、安装webpack-dev-server

本地依赖


```
npm install --save-dev webpack-dev-server
```

全局安装


```
npm install -g webpack-dev-server
```

#### 2、在webpack.config.js中配置devserver选项：

- contentBase : 默认webpack-dev-server会为根文件夹提供本地服务器，如果想为另外一个目录下的文件提供本地服务器，应该在这里设置其所在目录。
- port ：设置默认监听端口，如果省略，默认为”8080″
- inline : 设置为true，当源文件改变时会自动刷新页面
- colors ： 设置为true，使终端输出的文件为彩色的
- historyApiFallback ：在开发单页时非常有用，它依赖于HTML5 history API，如果设置为true，所有的跳转将指向index.html
- host ： 主机地址，如果是开发本机搭建，需要使用本机IP地址，否则会报错。

配置文件如下：


```
module.exports = {
    entry: __dirname + '/main.js',
    output: {
        path: __dirname + '/',
        filename: 'bundle.js'
    },
    devServer: {
        contentBase: './', //本地服务器所加载的页面所在的目录
        host: '192.168.1.158', //本地IP地址
        colors: true, //终端输出结果为彩色
        historyApiFallback: true, //不跳转
        inline: true, //实时刷新
        port: '3333' //端口号

    }
}
```

#### 3、在终端中输入命令，开启运行本地服务器。它不仅提供了服务器，还会监视我们的文件变化自动更新效果，但是它不是真正的打包，它类似于在内存中进行了打包。所以本地文件并没有变化。


```
webpack-dev-server
```

其实webpack构建服务器才是正道，如果你使用了webpack，那么尽量使用webpack-dev-server吧，这能大大提高您的工作效率。