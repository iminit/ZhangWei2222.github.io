---
title: delete和Vue.delete删除数组的区别
date: 2019-12-04 18:28:47
categories:
- Vue
comments: false
---



在unregisterModule函数(注销动态组件)中，发现了

```js
/* 从父级中删除 */
Vue.delete(parentState, path[path.length - 1]);
```

为啥使用Vue.delete删除呢？？

官方文档中是这样介绍的

```js
Vue.delete( target, propertyName/index )

// 用法：
// 删除对象的属性。如果对象是响应式的，确保删除能触发更新视图。这个方法主要用于避开 Vue 不能检测到属性被删除的限制，但是你应该很少会使用它。
```

与`delete`的区别就是：

1. delete只是被删除的元素变成了 empty/undefined 其他的元素的键值还是不变
2. Vue.delete 直接删除了数组 改变了数组的键值