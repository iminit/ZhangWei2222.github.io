---
title: Computed property XXX was assigned to but it has no setter
date: 2020-04-28 17:56:47
categories:
- Vue
comments: false
---

报错原因：
1. 组件中 v-model = 'xxx'，而 xxx 是 vuex state 中的某个变量
2. vuex 是单项流，v-model 是 vue 的双向绑定，但是在 computed 中只通过 get 获取参数值，**没有 set**，所以无法改变参数值

<!-- more -->

解决办法：

1. 在 computed 中添加 get 和 set

   ![image-20200428175953621](Computed property XXX was assigned to but it has no setter.assets/image-20200428175953621.png)

2. 将 v-model 改成 :value