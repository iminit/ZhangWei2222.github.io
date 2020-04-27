---
title: this.$router.push、replace、go 区别
date: 2020-04-27 10:04:47
categories:
- Vue
comments: false
---

1. `this.$router.push()`

   > 跳转到不同的 url，但这个方法会向 history 栈添加一个记录，点击后退会返回到上一个页面。

   <!-- more -->

   ```js
   this.$router.push({ path: '/user', query: {id : 1}})
   this.$router.push({ name: 'user', query: {id : 1}})
   ```

2. `this.$router.replace()`

   > 同样是跳转到指定的url，但是这个方法不会向 history 里面添加新的记录，点击返回，会跳转到上上一个页面。上一个记录是不存在的。

3. `this.$router.go()`

   > 相对于当前页面向前或向后跳转多少个页面,类似 `window.history.go(n)`。n 可为正数可为负数。

   ```js
   // 在浏览器记录中前进一步，等同于 history.forward()
   this.$router.go(1)
   
   //后退一步记录，等同于 history.bacK()
   this.$router.go(-1)
   ```

   