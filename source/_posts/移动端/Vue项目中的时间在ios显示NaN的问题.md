---
title: Vue项目中的时间在ios显示NaN的问题
date: 2020-04-17 16:52:47
categories:
- 移动端
comments: false
---

在毕设项目中，发现`2020-03-22 10:00:00`这种类型的时间，在ios显示为`NaN-NaN-NaN NaN:NaN:NaN`，但是在PC端调试没有任何问题，数据是正常到达页面的，为啥到了真机就NaN呢？查阅资料后发现，ios 不支持时间的`-`连接符，解决方法可以把`-`替换成`/`

```js
var date = '2020-03-22 12:00:00';
var format = date.replace(/-/g, '/');
var time= Date.parse(new Date(format));
```


