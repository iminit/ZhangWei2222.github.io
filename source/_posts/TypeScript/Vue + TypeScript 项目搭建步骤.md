---
title: Vue + TypeScript 项目搭建步骤
date: 2019-11-20 10:24:47
categories:
- Vue
- TypeScript
comments: false
---



## 前言

【2020.2.11】更新：决定在毕设项目中采用 Vue + TypeScript 搭建前端项目，在搭建的过程过，完善步骤、记录坑。

<!-- more -->



## 搭建

### 起手 Vue-Cli

1. npm 版本查看及升级

    ```shell
    $ npm -v # 查看版本
    $ npm install -g npm # 升级版本
    ```

2. 安装 Vue

    ```shell
    $ npm install vue # 最新稳定版
    $ npm install --global vue-cli # 安装vue-cli脚手架构建工具
    $ vue -V # 查看版本号
	```

3. 安装 Webpack

    ```shell
    $ npm install --global webpack
    $ npm install --global webpack-cli
    $ webpack -v
    ```

4. 新建项目

    ```shell
    $ vue init webpack 项目名字【选填】
    ```

5. 运行项目，访问 `http://localhost:8080/`

    ```sehll
    $ npm run dev
    ```

    

### Vue 引入 TypeScript

```shell
# 安装vue的官方插件
$ npm i vue-class-component vue-property-decorator --save

# ts-loader typescript 
$ npm i ts-loader typescript tslint tslint-loader tslint-config-standard --save-dev
```

**注意：安装完，删掉 `node_module` ，重新 `npm install`**

- **`vue-class-component`**：强化 `Vue` 组件，使用 `TypeScript`/装饰器 增强 `Vue` 组件
- **`vue-property-decorator`**：在 `vue-class-component` 上增强更多的结合 `Vue` 特性的装饰器
- **`ts-loader`**：`TypeScript` 为 `Webpack` 提供了 `ts-loader`，其实就是为了让`webpack`识别 `.ts .tsx`文件
- **`tslint-loader`跟`tslint`**：在`.ts .tsx`文件 约束代码格式（作用等同于`eslint`）
- **`tslint-config-standard`**：`tslint` 配置 `standard`风格的约束



### 配置 `webpack`

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

- `ts-loader` 会检索当前目录下的 `tsconfig.json` 文件，根据里面定义的规则来解析`.ts`文件（就跟`.babelrc`的作用一样）

- `tslint-loader` 作用等同于 `eslint-loader`



### 添加 `tsconfig.json`

在根路径下创建`tsconfig.json`文件

这里有一份参考的 `tsconfig.json` 配置，完成的配置请点击 [tsconfig.json](http://json.schemastore.org/tsconfig)：

```json
{
  // 编译选项
  "compilerOptions": {
    // 输出目录
    "outDir": "./output",
    // 是否包含可以用于 debug 的 sourceMap
    "sourceMap": true,
    // 以严格模式解析
    "strict": true,
    // 采用的模块系统
    "module": "esnext",
    // 如何处理模块
    "moduleResolution": "node",
    // 编译输出目标 ES 版本
    "target": "es5",
    // 允许从没有设置默认导出的模块中默认导入
    "allowSyntheticDefaultImports": true,
    // 将每个文件作为单独的模块
    "isolatedModules": false,
    // 启用装饰器
    "experimentalDecorators": true,
    // 启用设计类型元数据（用于反射）
    "emitDecoratorMetadata": true,
    // 在表达式和声明上有隐含的any类型时报错
    "noImplicitAny": false,
    // 不是函数的所有返回路径都有返回值时报错。
    "noImplicitReturns": true,
    // 从 tslib 导入外部帮助库: 比如__extends，__rest等
    "importHelpers": true,
    // 编译过程中打印文件名
    "listFiles": true,
    // 移除注释
    "removeComments": true,
    "suppressImplicitAnyIndexErrors": true,
    // 允许编译javascript文件
    "allowJs": true,
    // 解析非相对模块名的基准目录
    "baseUrl": "./",
    // 指定特殊模块的路径
    "paths": {
      "jquery": [
        "node_modules/jquery/dist/jquery"
      ]
    },
    // 编译过程中需要引入的库文件的列表
    "lib": [
      "dom",
      "es2015",
      "es2015.promise"
    ]
  }
}
```

#### 参考配置

```json
{
  "include": [
    "src/**/*"
  ],
  "exclude": [
    "node_modules"
  ],
  "compilerOptions": {
    "allowSyntheticDefaultImports": true,
    "experimentalDecorators": true,
    "allowJs": true,
    "module": "esnext",
    "target": "es5",
    "moduleResolution": "node",
    "isolatedModules": true,
    "lib": [
      "dom",
      "es5",
      "es2015.promise"
    ],
    "sourceMap": true,
    "pretty": true
  }
}
```



### 添加 `tslint.json`

在根路径下创建 `tslint.json` 文件：引入 `ts` 的 `standard` 规范

```json
{
    "extends": "tslint-config-standard",
    "globals": {
        "require": true
    },
    "defaultSeverity": "none" // 关掉 tslint，不然老是报错，好像现在 tslint 已经交给 eslint 来做，tslint 已经不维护了。注意配置后，要重新 run 才会生效
}
```



### 让 ts 识别 .vue

由于 `TypeScript` 默认并不支持 `*.vue` 后缀的文件，所以在 `vue` 项目中引入的时候需要创建一个 `vue-shim.d.ts` 文件，放在项目项目对应使用目录下，例如 `src/vue-shim.d.ts`

```ts
declare module "*.vue" {
  import Vue from "vue";
  export default Vue;
}
```

意思是告诉 `TypeScript *.vue` 后缀的文件可以交给 `vue` 模块来处理。

而在代码中导入 `*.vue` 文件的时候，需要写上 `.vue` 后缀。原因还是因为 `TypeScript` 默认只识别 `*.ts` 文件，不识别 `*.vue` 文件：

```
import Component from 'components/component.vue'
```



### 开始修改`App.vue`文件

1. 在`script` 标签上加上 `lang="ts"`, 意思是让`webpack`将这段代码识别为`typescript` 而非`javascript`
2. 修改 vue 组件的构造方式( 跟`react`组件写法有点类似, 详见[官方](https://cn.vuejs.org/v2/guide/typescript.html#基于类的-Vue-组件) )， 如下图
3. 用`vue-property-decorator`语法改造之前代码

<img src="https://segmentfault.com/img/bVXqRW?w=627&amp;h=495" alt="clipboard.png" style="zoom: 80%;" />

```vue
<template>
  <div id="app">
    <img src="./assets/logo.png">
    <router-view/>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import Component from 'vue-class-component'

@Component({})
export default class App extends Vue {
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

</style>
```

接下来用相同的方式修改HelloWorld.vue即可

或者（两种写法）

```js
// HelloWorld.vue
<script lang="ts">
import { Vue, Component } from 'vue-property-decorator'

@Component({})
export default class HelloWorld extends Vue {
  msg:string = 'Welcome to Your Vue.js AppAAA'
}
</script>
```



### 降级 `ts-loader` 至3.5.0
此时直接 `npm run dev` 是会报错的

> Module build failed: Error: You may be using an old version of webpack; please check you’re using at least version 4

这是由于 ts-loader 的版本较高的原因，所以需要将 ts-loader 降级到3.5.0

```shell
$ npm i ts-loader@3.5.0 --save-dev
```



### 修改 `main.ts`

再次 `npm run dev` ，可能会看到

> warning in ./src/main.ts
> [10, 1]: unused expression, expected an assignment or function call

那么看看 `main.ts`, 改为 ：

```vue
export default new Vue({
  el: '#app',
  router,
  components: { App },
  template: '<App/>'
})

```



### npm run dev

这个时候应该 毛闷题 了！



### 支持 es6 / es7

在 `tsconfig.json`中，添加对`es6 / es7`的支持，更多的配置请见[tsconfig - 编译选项](https://tslang.cn/docs/handbook/compiler-options.html)

```json
"lib": [
    "dom",
    "es5",
    "es6",
    "es7",
    "es2015.promise"
]
```



### 接入 Vuex

1. 安装 Vuex 

   ```shell
   npm i vuex vuex-class --save
   ```
   
   - **vuex-class** ：在 vue-class-component 写法中 绑定 vuex

2. 在 src 目录下，新建一个 `store` 目录，在目录下新建一个 `index.ts`，配置和之前写法一样

   ```ts
   import Vue from "vue";
   import Vuex from "vuex";
   
   Vue.use(Vuex);
   
   const store = new Vuex.Store({
     strict: true,
     state: {
       count: 1
     },
     getters: {
       getStateCount(state) {
         return state.count + 1;
       }
     },
     mutations: {
       add(state) {
         state.count = state.count + 1;
       },
       reduction(state) {
         state.count = state.count - 1;
       }
     },
     actions: {
       addFun(context) {
         context.commit("add");
       },
       reductionFun(context) {
         context.commit("reduction");
       }
     }
     // modules: { a: moduleA, b: moduleB }
   });
   
   export default store;
   ```

3. 在 `main.ts` 引入

   ```ts
   import Vue from "vue";
   import App from "./App.vue"; 
   import store from "./store"; // 添加
   
   Vue.config.productionTip = false;
   
   /* eslint-disable no-new */
   export default new Vue({
     el: "#app",
     router,
     store, // 添加
     components: { App },
     template: "<App/>"
   });
   
   ```




#### 引用例子

```ts
import Vue from 'vue'
import Component from 'vue-class-component'
import {
    State,
    Getter,
    Action,
    Mutation,
    namespace
} from 'vuex-class'

const ModuleGetter = namespace('path/to/module', Getter)

@Component
export class MyComp extends Vue {
    @State('foo') stateFoo
    @State(state => state.bar) stateBar
    @Getter('foo') getterFoo
    @Action('foo') actionFoo
    @Mutation('foo') mutationFoo
    @ModuleGetter('foo') moduleGetterFoo

    // If the argument is omitted, use the property name
    // for each state/getter/action/mutation type
    @State foo
    @Getter bar
    @Action baz
    @Mutation qux

    created () {
        this.stateFoo // -> store.state.foo
        this.stateBar // -> store.state.bar
        this.getterFoo // -> store.getters.foo
        this.actionFoo({ value: true }) // -> store.dispatch('foo', { value: true })
        this.mutationFoo({ value: true }) // -> store.commit('foo', { value: true })
        this.moduleGetterFoo // -> store.getters['path/to/module/foo']
    }
}
```



### 接入 SCSS

1. 安装

   ```shell
   npm install node-sass --save-dev
   npm install sass-loader --save-dev
   ```

2. 在 `./build/webpack.base.conf.js` 的 `module` 添加如下模块
   

    ```json
    {
      test: /\.scss$/,
      loaders: ["style", "css", "sass"]
    }
    ```

安装后，运行时报错

> Modele build failed: TypeError: this.getResolve is not a function at Object.loader...

这是因为当前sass的版本太高，webpack编译时出现了错误，这个时候只需要换成低版本的就行，很简单，如下，找到 `package.json` 文件，里面的 "sass-loader"的版本更换掉就行了。

可以将 **"sass-loader": "^8.0.0"，更换成 "sass-loader": "^7.3.1"**，最后重新 run 以下就好了



### 接入 Axios

1. 安装

   ```shell
   npm install --save axios vue-axios
   ```
   
2. `main.ts`引入

   ```ts
   import Vue from "vue";
   import axios from "axios"; // 添加
   import VueAxios from "vue-axios"; // 添加
   import App from "./App.vue";
   import router from "./router/index";
   import store from "./store";
   
   Vue.use(VueAxios, axios); // 添加
   Vue.config.productionTip = false;
   
   /* eslint-disable no-new */
   export default new Vue({
     el: "#app",
     router,
     store,
     components: { App },
     template: "<App/>"
   });
   
   ```

`Axios` 是一个库，并不是 vue 中的第三方插件，使用时不能通过 `Vue.use()` 安装插件

在 `mian.js` 中引用 `axios，vue-axios`，通过全局方法 `Vue.use() `使用插件，就相当于调用 `install` 方法



### 接入 Element

1. 安装

   ```shell
   npm i element-ui -S
   ```
   
2. `main.ts`

   ```ts
   import Vue from "vue";
   import ElementUI from "element-ui"; // 添加
   import "element-ui/lib/theme-chalk/index.css"; // 添加
   import App from "./App.vue";
   
   Vue.use(ElementUI); // 添加
   Vue.config.productionTip = false;
   
   /* eslint-disable no-new */
   export default new Vue({
     el: "#app",
     router,
     store,
     components: { App },
     template: "<App/>"
   });
   
   ```

   

### 接入 Express + NodeJs

1. 安装

    ```shell
    npm install express cors body-parser mysql
    ```

2. 新建一个 `server.ts`

    ```ts
    "use strict";
    /* 引入express框架 */
    const express = require("express");
    const app = express();
    
    /* 引入cors */
    const cors = require("cors");
    app.use(cors());
    
    /* 引入body-parser */
    const bodyParser = require("body-parser");
    app.use(bodyParser.json());
    app.use(
      bodyParser.urlencoded({
        extended: false
      })
    );
    
    /* 引入mysql */
    const mysql = require("mysql");
    const conn = mysql.createConnection({
      host: "101.133.132.172",
      user: "root",
      password: "123456",
      database: "hostel",
      multipleStatements: true
    });
    conn.connect();
    
    /* 监听端口 */
    app.listen(4444, () => {
      console.log("——————————服务已启动——————————");
    });
    
    app.get("/", (req, res) => {
      res.send('<p style="color:red">服务已启动</p>');
    });
    ```



### 使用 nodemon 热加载 NodeJs文件

```shell
npm install -g  nodemon
```

安装后只要`nodemon server.ts`即可

运行时，发现有报错，需要安装一个`@type/node`，以及在 `tsconfig.json` 的`compilerOptions`配置`"types": ["node"]`，跟着提示走就好了。

但是可能还会报：
> Type error: Cannot compile namespaces when the '--isolatedModules' flag is provided.

那么，在 `tsconfig.json` 的`compilerOptions`配置`"isolatedModules": false`，就没问题



### 取消 eslint 校验

实在是烦...先取消了

在根目录创建`vue.config.js`

```js
module.exports = {
  lintOnSave: false
}
```



### 统一代码格式

为了快速编码，决定启用 保存后自动格式化，需要在 `首选项-设置` 添加

```json
// #每次保存的时候自动格式化
"editor.formatOnSave": true,
```

这里有个坑，之前开启了 eslint 校验，又开了 prettier ，vetur，导致格式化和报错一直打架，因为方便完成自己的毕设，所以关掉了 eslint，但是格式化又在打架，查阅，发现 prettier 在存在 vetur 的前提下，并不生效，所以我决定把 prettier 先关掉！

下面只要注重 Vetur 的配置就好了，同样在 `首选项-设置`  找  `Vetur`，想要改啥配置，百度一下吧~

#### 查阅资料

[vscode中eslint插件的配置（prettier配置无效）](https://www.jb51.net/article/169682.htm)

[使用ESLint+Prettier来统一前端代码风格](https://segmentfault.com/a/1190000015315545)



## 插件使用指南

### vue-class-component

[vue-class-component](https://github.com/vuejs/vue-class-component) 对 `Vue` 组件进行了一层封装，让 `Vue` 组件语法在结合了 `TypeScript` 语法之后更加扁平化：

```vue
<template>
  <div>
    <input v-model="msg">
    <p>msg: {{ msg }}</p>
    <p>computed msg: {{ computedMsg }}</p>
    <button @click="greet">Greet</button>
  </div>
</template>

<script lang="ts">
  import Vue from 'vue'
  import Component from 'vue-class-component'

  @Component
  export default class App extends Vue {
    // 初始化数据
    msg = 123

    // 声明周期钩子
    mounted () {
      this.greet()
    }

    // 计算属性
    get computedMsg () {
      return 'computed ' + this.msg
    }

    // 方法
    greet () {
      alert('greeting: ' + this.msg)
    }
  }
</script>
```

上面的代码跟下面的代码作用是一样的

```javascript
export default {
  data () {
    return {
      msg: 123
    }
  }

  // 声明周期钩子
  mounted () {
    this.greet()
  }

  // 计算属性
  computed: {
    computedMsg () {
      return 'computed ' + this.msg
    }
  }

  // 方法
  methods: {
    greet () {
      alert('greeting: ' + this.msg)
    }
  }
}
```

### vue-property-decorator

[vue-property-decorator](https://github.com/kaorun343/vue-property-decorator) 是在 `vue-class-component` 上增强了更多的结合 `Vue` 特性的装饰器，新增了这 7 个装饰器：

- `@Emit`
- `@Inject`
- `@Model`
- `@Prop`
- `@Provide`
- `@Watch`
- `@Component` (从 `vue-class-component` 继承)

在这里列举几个常用的`@Prop/@Watch/@Component`, 更多信息，详见[官方文档](https://github.com/kaorun343/vue-property-decorator)

```
import { Component, Emit, Inject, Model, Prop, Provide, Vue, Watch } from 'vue-property-decorator'

@Component
export class MyComponent extends Vue {
  
  @Prop()
  propA: number = 1

  @Prop({ default: 'default value' })
  propB: string

  @Prop([String, Boolean])
  propC: string | boolean

  @Prop({ type: null })
  propD: any

  @Watch('child')
  onChildChanged(val: string, oldVal: string) { }
}
```

上面的代码相当于：

```
export default {
  props: {
    checked: Boolean,
    propA: Number,
    propB: {
      type: String,
      default: 'default value'
    },
    propC: [String, Boolean],
    propD: { type: null }
  }
  methods: {
    onChildChanged(val, oldVal) { }
  },
  watch: {
    'child': {
      handler: 'onChildChanged',
      immediate: false,
      deep: false
    }
  }
}
```



## 参考链接

[vue + typescript 项目起手式](https://segmentfault.com/a/1190000011744210)

[vue + typescript 进阶篇](https://segmentfault.com/a/1190000011878086)



