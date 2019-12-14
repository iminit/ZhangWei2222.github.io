---
title: element实现导航
date: 2018-08-01 15:20:12
categories:
- Vue
tags:
- Element
comments: false
---

```
<template>
  <div class="app">
    <el-header>
      <el-menu :default-active="$route.path" router class="el-menu-demo" mode="horizontal" @select="handleSelect" background-color="#545c64"
  text-color="#fff"
  active-text-color="#ffd04b">
        <el-menu-item index="/Help">帮助</el-menu-item>
        <el-menu-item index="/Central-summary">中心概括</el-menu-item>
        <el-menu-item index="/Flying-Eagle-Academy">飞鹰学苑</el-menu-item>
        <el-menu-item index="/Strategy-comparison">策略对比</el-menu-item>
        <el-menu-item index="/Strategy-backtest">策略回测</el-menu-item>
        <el-menu-item index="/Combined-configuration">组合配置</el-menu-item>
      </el-menu>
      <div class="line"></div>
    </el-header>
    <el-main>
      <router-view></router-view>
    </el-main>
  </div>
</template>
<script>
  export default {
    data() {
      return {
        activeIndex: "1"
      };
    }
  };
</script>
```


vue中使用element实现导航需要注意三个方面

### 1.启用vue-router

在el-menu中添加 router

### 2.刷新页面后，对应menu高亮

更改 :default-active="$route.path"

### 3.添加各路由

在el-menu-item中的index属性直接书写的路由