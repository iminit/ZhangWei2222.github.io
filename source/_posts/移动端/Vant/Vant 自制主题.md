---
title: Vant 自制主题
date: 2020-02-15 14:44:47
categories:
- 移动端
tags:
- Vant 框架
comments: false
---



尽管官方有教程，但是还是踩了坑，记录记录！【本次教程针对 vue-cli 2.x】
<!-- more -->

1. 安装 插件

   ```shell
   npm i babel-plugin-import -D
   ```


2. `main.ts` 引入

   ```ts
   import 'vant/lib/index.less'; // 注意是less
   ```

3. `build / utils.js` 配置 `generateLoaders`

   ```js
   less: generateLoaders('less', {
       modifyVars: {
           // 定义自己的文件覆盖，采用绝对路径
           'hack': `true; @import "${path.join(__dirname,'../src/common/style/ResetVant.less')}";`
       }
   }),
       sass: generateLoaders('sass', {
           indentedSyntax: true
       }),
   ```

4. `.babelrc`  配置

   ```json
   "plugins": ["transform-vue-jsx", "transform-runtime", [
       "import", {
           "libraryName": "vant",
           "libraryDirectory": "es",
           "style": true
       },
       "vant"
   ]]
   ```

5. 新建一个 `.less` 文件，复制配置文件，更改相关样式，重新编译，即可生效
![img](https://img-blog.csdnimg.cn/20190831161216366.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2JhaWR1XzQxNjAxMDQ4,size_16,color_FFFFFF,t_70)