---
title: vue+ts基础
date: 2019-11-20 10:24:47
categories:
- Vue
comments: false
---

#### 1、安装插件

<!-- more -->

```js
// 安装vue的官方插件
npm i vue-class-component vue-property-decorator --save

// ts-loader typescript 
npm i ts-loader typescript tslint tslint-loader tslint-config-standard --save-dev
```

- **`vue-class-component`**：强化 `Vue` 组件，使用 `TypeScript`/装饰器 增强 `Vue` 组件
- **`vue-property-decorator`**：在 `vue-class-component` 上增强更多的结合 `Vue` 特性的装饰器
- **`ts-loader`**：`TypeScript` 为 `Webpack` 提供了 `ts-loader`，其实就是为了让`webpack`识别 `.ts .tsx`文件
- **`tslint-loader`跟`tslint`**：在`.ts .tsx`文件 约束代码格式（作用等同于`eslint`）
- **`tslint-config-standard`**：`tslint` 配置 `standard`风格的约束

#### 2、配置 `webpack`

`./build/webpack.base.conf.js`

配置三个地方：`entry`、`resolve`、`module`

```js
entry: {
  app: './src/main.ts'
}

resolve: {
    extensions: ['.js', '.vue', '.json', '.ts'],
    alias: {
      '@': resolve('src')
    }
}

module: {
  rules: [
    {
      test: /\.(js|vue)$/,
      loader: 'eslint-loader',
      enforce: 'pre',
      include: [resolve('src'), resolve('test')],
      options: {
        formatter: require('eslint-friendly-formatter')
      }
    },
// 从这里复制下面的代码就可以了
    {
      test: /\.ts$/,
      exclude: /node_modules/,
      enforce: 'pre',
      loader: 'tslint-loader'
    },
    {
      test: /\.tsx?$/,
      loader: 'ts-loader',
      exclude: /node_modules/,
      options: {
        appendTsSuffixTo: [/\.vue$/],
      }
    },
// 复制以上的
  }
}
```

#### 3、添加 `tsconfig.json`

在根路径下创建`tsconfig.json`文件