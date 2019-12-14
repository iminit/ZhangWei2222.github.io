---
title: Keep-alive源码分析
date: 2019-09-10 16:13:47
categories:
- 源码分析
tags:
- Vue
comments: false
---

## 前言

Vue的内置组件，能在组件切换过程中将状态保留在内存中，防止重复渲染dom，达到**缓存**的效果

- include: 字符串或正则表达式。只有匹配的组件会被缓存。
- exclude: 字符串或正则表达式。任何匹配的组件都不会被缓存。



## 用法

keep-alive 需要配合 router-view 使用

```js
<template>
  <div id="app">
    <!--缓存想要缓存的页面，实现后退不刷新-->
    <!--加上v-if的判断，可以自定义想要缓存的组件，自定义在router里面-->
    <keep-alive>
      <router-view v-if="$route.meta.keepAlive"></router-view>
    </keep-alive>
    <router-view v-if="!$route.meta.keepAlive"></router-view>
    
    <!--这里是其他的代码-->
  </div>
</template>

//在 router 文件上加 meta 判断
import Vue from 'vue'
import Router from 'vue-router'
 
Vue.use(Router)
export default new Router({
    {//home会被缓存
        path:"/home",
        component:home,
        meta:{keepAlive: true}
    }
    {//hello不会被缓存
        path:"/hello",
        component:hello,
        meta:{keepAlive: false}
    }
})
```



## 生命周期

- activated

- deactivated

因为 keep-alive 会将组件**保存在内存**中，并**不会销毁以及重新构建**，所以不会重新调用组件的 created等 方法，需要用 activated和deactivated 得知当前组件**是否处于活动状态**



## 原理

#### created 与 destroyed 钩子

created钩子 会**创建一个cache对象**，用来**作为缓存容器**，保存**vnode节点**

```js
created () {
    /* 缓存对象 */
    this.cache = Object.create(null)
},
```

destroyed钩子 则在**组件被销毁**的时候**清除cache缓存中**的所有组件实例

```js
/* destroyed钩子中销毁所有cache中的组件实例 */
destroyed () {
    for (const key in this.cache) {
        pruneCacheEntry(this.cache[key])
    }
},
```



#### render

1. 通过 **getFirstComponentChild** 获取**第一个子组件**，
2. 获取该组件的 **name** （存在组件名则直接使用组件名，否则会使用**tag**）
3. 将这个 name 通过 **include 与 exclude 属性**进行匹配
4. 匹配**不成功**（说明不需要进行缓存）则不进行任何操作**直接返回vnode**

```js
render () {
    /* 得到slot插槽中的第一个组件 */
    const vnode: VNode = getFirstComponentChild(this.$slots.default)

    const componentOptions: ?VNodeComponentOptions = vnode && vnode.componentOptions
    if (componentOptions) {
        // check pattern
        /* 获取组件名称，优先获取组件的name字段，否则是组件的tag */
        const name: ?string = getComponentName(componentOptions)
        /* name不在inlcude中或者在exlude中则直接返回vnode（没有取缓存） */
        if (name && (
        (this.include && !matches(this.include, name)) ||
        (this.exclude && matches(this.exclude, name))
        )) {
            return vnode
        }
        const key: ?string = vnode.key == null
        // same constructor may get registered as different local components
        // so cid alone is not enough (#3269)
        ? componentOptions.Ctor.cid + (componentOptions.tag ? `::${componentOptions.tag}` : '')
        : vnode.key
        /* 如果已经做过缓存了则直接从缓存中获取组件实例给vnode，还未缓存过则进行缓存 */
        if (this.cache[key]) {
            vnode.componentInstance = this.cache[key].componentInstance
        } else {
            this.cache[key] = vnode
        }
        /* keepAlive标记位 */
        vnode.data.keepAlive = true
    }
    return vnode
}
```

检测 include 与 exclude属性匹配的函数很简单，include 与 exclude 属性 **支持字符串**如"a,b,c"这样组件名以逗号隔开的情况以及**正则表达式**。matches通过这两种方式分别检测是否匹配当前组件。

```js
/* 检测name是否匹配 */
function matches (pattern: string | RegExp, name: string): boolean {
  if (typeof pattern === 'string') {
    /* 字符串情况，如a,b,c */
    return pattern.split(',').indexOf(name) > -1
  } else if (isRegExp(pattern)) {
    /* 正则 */
    return pattern.test(name)
  }
  /* istanbul ignore next */
  return false
}
```

5. 根据 **key 在this.cache中查找**，如果存在则说明之前已经缓存过了，直接将缓存的vnode的组件实例**覆盖**到目前的vnode上面
6. 否则将 vnode 存储在 cache 中，再返回 vnode

```js
if (this.cache[key]) {
    vnode.componentInstance = this.cache[key].componentInstance
} else {
    this.cache[key] = vnode
}
```



#### watch

1. 用 watch 来监听 include 与 exclude 这两个属性的改变，**在改变的时候修改cache缓存中的缓存数据**

```js
watch: {
    /* 监视include以及exclude，在被修改的时候对cache进行修正 */
    include (val: string | RegExp) {
        pruneCache(this.cache, this._vnode, name => matches(val, name))
    },
    exclude (val: string | RegExp) {
        pruneCache(this.cache, this._vnode, name => !matches(val, name))
    }
},
```

2. **遍历**cache中的所有项，如果**不符合**filter指定的规则的话，则会执行**pruneCacheEntry**
3. **pruneCacheEntry**则会调用组件实例的$**destroy**方法来将组件**销毁**

```
/* 修正cache */
function pruneCache (cache: VNodeCache, current: VNode, filter: Function) {
  for (const key in cache) {
    /* 取出cache中的vnode */
    const cachedNode: ?VNode = cache[key]
    if (cachedNode) {
      const name: ?string = getComponentName(cachedNode.componentOptions)
      /* name不符合filter条件的，同时不是目前渲染的vnode时，销毁vnode对应的组件实例（Vue实例），并从cache中移除 */
      if (name && !filter(name)) {
        if (cachedNode !== current) {
          pruneCacheEntry(cachedNode)
        }
        cache[key] = null
      }
    }
  }
} 

/* 销毁vnode对应的组件实例（Vue实例） */
function pruneCacheEntry (vnode: ?VNode) {
  if (vnode) {
    vnode.componentInstance.$destroy()
  }
}
```



## 总结

Vue.js内部将DOM节点抽象成了一个个的 VNode节点，keep-alive 组件的缓存也是**基于VNode节点**的而不是直接存储DOM结构

它将满足条件（include与exclude）的组件在cache对象中缓存起来，在需要重新渲染的时候再将vnode节点 从 cache对象中 取出并渲染