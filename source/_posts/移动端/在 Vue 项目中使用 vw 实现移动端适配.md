---
title: 在 Vue 项目中使用 vw 实现移动端适配
date: 2020-02-14 10:16:47
categories:
- 移动端
---



> 由于`viewport`单位得到众多浏览器的兼容，`lib-flexible`这个过渡方案已经可以放弃使用，不管是现在的版本还是以前的版本，都存有一定的问题。建议大家开始使用`viewport`来替代此方案。`vw`的兼容方案可以参阅《[如何在Vue项目中使用vw实现移动端适配](https://www.w3cplus.com/mobile/vw-layout-in-vue.html)》一文。

之前一直没有尝试使用 vw 进行移动端适配，这次希望在毕设项目中运用，记录一番步骤，具体的插件用途可查阅参考文章~

<!-- more -->



## 安装 PostCSS 插件

```shell
npm i postcss-aspect-ratio-mini postcss-px-to-viewport postcss-write-svg postcss-cssnext postcss-viewport-units cssnano --S   
```



## 配置插件

在`.postcssrc.js`文件对新安装的PostCSS插件进行配置：

```js
// https://github.com/michael-ciniawsky/postcss-load-config

module.exports = {
  "plugins": {
    "postcss-import": {},
    "postcss-url": {},
    "postcss-aspect-ratio-mini": {},
    "postcss-write-svg": {
      utf8: false
    },
    "postcss-cssnext": {},
    "postcss-px-to-viewport": {
      viewportWidth: 750, // 视窗的宽度，对应的是我们设计稿的宽度，一般是750
      viewportHeight: 1334, // 视窗的高度，根据750设备的宽度来指定，一般指定1334，也可以不配置
      unitPrecision: 3, // 指定`px`转换为视窗单位值的小数位数（很多时候无法整除）
      viewportUnit: 'vw', // 指定需要转换成的视窗单位，建议使用vw
      selectorBlackList: ['.ignore', '.hairlines'], // 指定不转换为视窗单位的类，可以自定义，可以无限添加,建议定义一至两个通用的类名
      minPixelValue: 1, // 小于或等于`1px`不转换为视窗单位，你也可以设置为你想要的值
      mediaQuery: false // 允许在媒体查询中转换`px`
    },
    "postcss-viewport-units": {
      filterRule: rule => rule.nodes.findIndex(i => i.prop === 'content') === -1,
    },
    "cssnano": {
      preset: "advanced",
      autoprefixer: false,
      "postcss-zindex": false
    }
  }
}
```

> **特别声明：**由于`cssnext`和`cssnano`都具有`autoprefixer`,事实上只需要一个，所以把默认的`autoprefixer`删除掉，然后把`cssnano`中的`autoprefixer`设置为`false`。



**特别声明：**

1. 由于 `cssnext` 和 `cssnano` 都具有 `autoprefixer` ,事实上只需要一个，所以把默认的 `autoprefixer` 删除掉，然后把 `cssnano` 中的 `autoprefixer` 设置为 `false`

2. `postcss-viewport-units`  修改 content ，并不能很好的兼容，会出现以下警告:        

   ```        shell
   (Emitted value instead of an instance of Error) postcss-viewport-units: ... a:after' already has a 'content' property, give up to overwrite it. 
   ```

    **解决办法**: 如果没有版本需求，可以考虑去掉;  或者进行过滤，具体如下:

    ```js
    "postcss-viewport-units": {
        filterRule: rule => rule.nodes.findIndex(i => i.prop === 'content') === -1,
    },
    ```



## 使用

在不想要把 `px` 转换为 `vw` 的时候，首先在对应的元素（`html`）中添加配置中指定的类名 `.ignore` 或 `.hairlines` (`.hairlines` 一般用于设置 `border-width:0.5px` 的元素中)：

```html
<div class="box ignore"></div>
```

写 CSS 时

```css
.ignore {
    margin: 10px;
    background-color: red;
}
.box {
    width: 180px;
    height: 300px;
}
.hairlines {
    border-bottom: 0.5px solid red;
}
```

编译出来的 CSS

```css
.box {
    width: 24vw;
    height: 40vw;
}
.ignore {
    margin: 10px; /*.box元素中带有.ignore类名，在这个类名写的`px`不会被转换*/
    background-color: red;
}
.hairlines {
    border-bottom: 0.5px solid red;
}
```

上面解决了`px`到`vw`的转换计算。那么在哪些地方可以使用`vw`来适配我们的页面。根据相关的测试：

- 容器适配，可以使用`vw`
- 文本的适配，可以使用`vw`
- 大于`1px`的边框、圆角、阴影都可以使用`vw`
- 内距和外距，可以使用`vw`



## 参考文章

[如何在Vue项目中使用vw实现移动端适配(转)](https://www.jianshu.com/p/1f1b23f8348f)