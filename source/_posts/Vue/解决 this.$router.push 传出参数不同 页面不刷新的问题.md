---
title: 解决 this.$router.push 传出参数不同 页面不刷新的问题
date: 2020-04-27 11:39:47
categories:
- Vue
comments: false
---

在页面的watch中，监听$router的变化

```js
watch: {
    $route (to, from) {
        this.$router.go(0)
    }
}
```


其中 `this.$router.go(0)`为刷新页面，但此方法可能在 Safari 中无法实现，建议使用 JS 原生的方法：`window.location.reload()`

