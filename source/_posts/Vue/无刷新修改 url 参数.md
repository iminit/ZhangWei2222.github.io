---
title: vue-无刷新修改 url 参数
date: 2020-04-27 10:16:47
categories:
- Vue
comments: false
---



```js
import merge from 'webpack-merge'；

// 修改原有参数        
this.$router.push({
    query:merge( this.$route.query, { 'maxPrice': '630' })
})

// 新增一个参数：
this.$router.push({
    query:merge( this.$route.query, { 'addParams': '新增参数' })
})

// 替换所有参数：
this.$router.push({
    query:merge( {}, { 'maxPrice': '630' })
})
```

