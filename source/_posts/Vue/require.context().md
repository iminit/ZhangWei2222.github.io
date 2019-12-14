---
title: Vue-页面需要引入多个组件技巧
date: 2019-11-21 09:38:47
categories:
- Vue
comments: false
---

**场景：页面需要引入多个组件**

```js
const files = require.context('@/components/home', false, /\.vue$/)
const modules = {}

function getFloorConfigName(path) {
    return path.match(/([\w-]+)\.vue/)[1];
} //([\w-]+) ： 匹配数字和字母下划线，中划线的多个字符

files.keys().forEach(key => {
  modules[getFloorConfigName(key)] = files(key).default || files(key)
})
components:modules
```

