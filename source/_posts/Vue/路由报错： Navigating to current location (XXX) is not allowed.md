---
title: 路由报错： Navigating to current location (XXX) is not allowed
date: 2020-04-28 11:36:47
categories:
- Vue
comments: false
---

报错原因：在路由中添加了相同的路由

解决办法：**重写路由的 push 方法**

在 `src/router/index.js` 里面 `import Router from 'vue-router'` 下面写入下面方法即可

```js
/**
 * 重写路由的push方法
 */
const routerPush = Router.prototype.push
Router.prototype.push = function push(location) {
  return routerPush.call(this, location).catch(error=> error)
}
```