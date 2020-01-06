---
title: webpack 路径问题
date: 2019-12-05 18:23:36
categories:
- 打包工具
tags:
- Webpack
comments: false
---



在分析 Vuex 源码的时候，为了方便调试，引入了本地的 Vuex 包，并更改 Webpack 配置的路径，其中有个  `vuex$ vue$`的路径配置挺有意思，为什么要配这个呢？

<!-- more -->

```js
// 在 webpack.base.conf.js中
resolve: {
  extensions: [".js", ".vue", ".json"],
    alias: {
      vue$: "vue/dist/vue.esm.js",
        vuex$: resolve("src/vuex/index.esm.js"),
          vuex: resolve("src/vuex"),
            "@": resolve("src")
    }
},
  // 为啥？？
```

首先看`vuex/src/index.js && index.esm.js`这两个文件，可以看出index.ems.js只多了export对象

我们代码里是这样：`import { mapState, mapGetters, mapActions } from "vuex";`

就得使用`index.esm.js`，因为es6的写法

```js
// 第一组
export default function crc32() { // 输出
  // ...
}
 
import crc32 from 'crc32'; // 输入
 
// 第二组
export function crc32() { // 输出
  // ...
};
 
import {crc32} from 'crc32'; // 输入
```

所以要获取mapState等函数就要使用vuex$进行精准查找到index.ems.js文件。但是我们想要vuex文件夹的其他文件，就需要再定义vuex，才能找到，比如

```js
import devtoolPlugin from "vuex/plugins/devtool.js";
console.log(devtoolPlugin);
```

所以需要配置 vuex$和vuex两个路径，当然，也可以在`import Vuex form '相对路径'`