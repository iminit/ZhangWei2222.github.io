---
title: 【分享】vue原理分析
date: 2019-12-03 15:17:47
categories:
- Vue
comments: false
---



## 前言

海南分享了vue的核心原理，从源码出发回答经常遇到的一些问题，比如为啥不能检测到对象/数组属性的变化等。记录几个get到的小点

1. 为啥多次赋值操作，页面只显示最后一个值？？？
   - vue有个数据队列，根据dom的deep算法，进行更新
2. 为啥this.data.num可以用this.num表示？？？
   - object.defineProperty又劫持了data，并且把它的属性扩展到vue的实例中，所以可以直接使用this.num；methods也一样
3. vue的生命周期为啥是那个顺序？？？
   - 源码中就规定了这样的顺序，首先整出data，然后created，initwatch，complie(编译挂载dom)，mounted(可以操作dom)
4. 性能优化：如果之后不会用到的属性不要放在data里面，因为在data里面的属性都会被劫持，影响性能。我们可以在data外用const接收，下面一样能够使用到变量。vue3.0将没有这个问题，用了某个属性才会被劫持(**海南补充：性能优化是3.0是用proxy实现，用了WeakMap，存储data与劫持后的对象的映射关系**)
5. 了解了一下computed的大概原理：比如 `c(){return a+b}` c这个属性会被识别为`computed`，然后有一系列的回调处理，主要是收集a 和 b的依赖，达到a、b被set后，c能监听到，然后变化
6. vuex为什么能管理vue？？？
   - vuex会new一个vue实例，然后对state进行劫持，靠vue实例的原理达到更新。所以此时有两个vue实例，两个dep(依赖收集)，如果不是大的项目就不太必要使用vuex，两个vue实例性能会有影响

以上小点是当时听分享看ppt得出的结论，或许会有偏差，自己也记录了一些不太懂的地方，之后还是需要阅读vue源码，再到回来看看~

<!-- more -->

下面补充分享内容~干货！



## 开始 VueJs 原理分析

Vue会遇到的一些问题

- 点击修改不生效的例子

```vue
<template>
<div>
  <ul>
    <li v-for="(item, index) in arr" :key="index">
      {{item.a}}
      <button @click="change(index)">修改数组元素</button>
      <button @click="change2(index)">修改数组元素内的属性值</button>
  </li>
  </ul>
  </div>
</template>
<script>
  export default {
    data () {
      return {
        arr: [
          {a: 111, b: 222},
          {a: 111, b: 222},
          {a: 111, b: 222},
          {a: 111, b: 222},
        ],
        num: 1
      }
    },
    methods: {
      change(index){
        this.arr[index] = {a: 444}
        // this.$set(this.arr,index,{a: 555})
      },
      change2(index){
        this.num++
        this.arr[index].a = this.num
      }
    }
  }
</script>
```

[弹窗例子](https://run.iviewui.com/w9xcWmI3)

- 改变数组或者对象中的值，但是视图没有更新，为什么用set可以，[看源码](https://github.com/vuejs/vue/blob/dev/src/core/observer/index.js) [数组方法](https://github.com/vuejs/vue/blob/dev/src/core/observer/array.js)
- 使用refs调用弹窗中的方法，报错，未找到该方法
- 多次赋值，为什么页面只显示最后的值，this.a = '111'; this.a = '222'[看源码](https://github.com/vuejs/vue/blob/dev/src/core/observer/scheduler.js)
- nextTick还是setTimeout[看源码](https://github.com/vuejs/vue/blob/dev/src/core/util/next-tick.js)
- v-show,v-if到底用哪个好
- v-html里写插值为什么不生效
- ...



## 开始原理

百度“vue原理”这个关键词，90%的几率你会看到类似的一句话：

“vue使用Object.defineProperty对数据进行getter与setter的转化，但由于ie8及以下的浏览器不支持Object.defineProperty这个属性，所以vue不兼容ie8及以下的浏览器”

那么，Object.defineProperty到底是个什么东西呢？

[mdn的解释](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)

- value：属性对应的值,可以使任意类型的值，默认为undefined
- writable：属性的值是否可以被重写
- enumerable：此属性是否可以被枚举
- configurable：是否可以删除目标属性或是否可以再次修改属性的特性
- get：获取对象值的时候触发
- set：设置对象值的时候触发

```js
var data = {
  name: 'lhl'
}

Object.keys(data).forEach((key) => {
  Object.defineProperty(data, key, {
    enumerable:true,
    configurable:true,
    get(){
      console.log('get');
    },
    set(val){
      console.log('监听到数据发生了变化');
    }
  })
})；
data.name //控制台会打印出 “get”
data.name = 'hxx' //控制台会打印出 "监听到数据发生了变化"
```



## 发布订阅

```js
var salesOffices = {}; // 定义售楼处
salesOffices.clientList = []; // 缓存列表，存放订阅者的回调函数
salesOffices.listen = function( fn ){ // 增加订阅者
  this.clientList.push( fn ); // 订阅的消息添加进缓存列表
};
salesOffices.trigger = function(){ // 发布消息
  for( var i = 0, fn; fn = this.clientList[ i++ ]; ){
    fn.apply( this, arguments ); // (2) // arguments 是发布消息时带上的参数
  }
};
salesOffices.listen( function( price, squareMeter ){ // 小明订阅消息
  console.log( '价格= ' + price );
  console.log( 'squareMeter= ' + squareMeter );
});

salesOffices.listen( function( price, squareMeter ){ // 小红订阅消息
  console.log( '价格= ' + price );
  console.log( 'squareMeter= ' + squareMeter );
});

salesOffices.trigger( 2000000, 88 ); // 输出：200 万，88 平方米
salesOffices.trigger( 3000000, 110 ); // 输出：300 万，110 平方米
```



## 看看核心的图

![img](file:////Users/wei/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image001.png)![img](file:////Users/wei/Library/Group%20Containers/UBF8T346G9.Office/TemporaryItems/msohtmlclip/clip_image002.jpg)



## vue的三个核心类

- Observer：用来劫持并监听所有属性，如果有变动的，就通知订阅者
- Dep：Observer与Watcher的中转站，对依赖进行收集并通知变化
- Watcher：可以收到属性的变化通知并执行相应的函数，从而更新视图



### Observer

Observer的核心是通过Obeject.defineProperty()来监听数据的变动，这个函数内部可以定义setter和getter，每当数据发生变化，就会触发setter。这时候Observer就要通知订阅者，订阅者就是Watcher。



### Watcher

Watcher订阅者作为Observer和Compile之间通信的桥梁，主要做的事情是：

在自身实例化时往属性订阅器(dep)里面添加自己 自身必须有一个update()方法 待属性变动dep.notice()通知时，能调用自身的update()方法，并触发Compile中绑定的回调



### Compile

Compile主要做的事情是解析模板指令，将模板中的变量替换成数据，然后初始化渲染页面视图，并将每个指令对应的节点绑定更新函数，添加监听数据的订阅者，一旦数据有变动，收到通知，更新视图。



## 手摸手写一个简易版vuejs帮助理解以下几点

- 实现数据双向绑定
- 实现事件监听
- 实现生命周期
- 实现watch

```vue
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <!-- <script src="https://cdn.bootcss.com/vue/2.5.16/vue.js"></script> -->
  </head>
  <body>
    <div id="app">
      <p>{{msg}}</p>
      <p>{{msg}}</p>
      <p>{{msg}}</p>
      <p>{{num}}</p>
      <input type="text" v-model="num">
      <button @click="change">change</button>
    </div>



    <script>
      // Dep的作用是作为Observer与Watcher的中转站，接收Observer发出的变化通知，转告给Watcher并进行变化后相应的处理
      class Dep{
        constructor(){
          // subs是一个依赖池，里面放了所有的Watcher
          this.subs = []
        }
        addSub(watcher){
          this.subs.push(watcher)
        }
        // notify用来通知变化给Watcher
        notify(val, oldVal){
          for (const watcher of this.subs) {
            watcher.update(val, oldVal)
          }
        }
      }
      // Watcher的作用是接收变化的通知，并且执行相应的函数
      class Watcher{
        constructor(vm, key, cb){
          this.vm = vm
          this.key = key
          this.cb = cb
          this.get()
        }
        // 当数据发生变化的时候就会触发这个事件
        update(val, oldVal){
          this.cb.call(this.vm , val, oldVal)
        }
        // 当我new的时候就触发这个函数，把自己丢进Dep中
        get(){
          Dep.target = this
          let value = this.vm[this.key]
          Dep.target = null
        }
      }
      // Observer的作用将数据进行getter与setter的转化，使其变为可响应的
      class Observer{
        constructor(data){
          this.data = data
          this.init(this.data)
        }
        init(data){
          // init方法使用核心的Object.defineProperty进行实际的转化操作
          if( !data || typeof(data) !== 'object') return
          for (const key in data) {
            this.defiReactive(data, key, data[key])
            // 递归调用数据层级别比较深的数据
            this.init(data[key])
          }
        }
        defiReactive(data, key, val){
          let oldVal = val
          let dep = new Dep()
          Object.defineProperty(data, key, {
            enumerable:true,
            configurable:true,
            get(){
              // console.log('get');
              // 为保证不会重复添加依赖，使用Dep.target进行判断
              if(Dep.target){
                dep.addSub(Dep.target)
              }
              return oldVal
            },
            set(val){
              // 进行简单的值对比，相同就不触发后续操作
              if(oldVal === val) return
              dep.notify(val, oldVal)
              // console.log(val, '监听到数据发生了变化');
              oldVal = val
            }
          })
        }
      }
      // Complie是用于进行模板解析的（这部分和源码完全不同，纯粹是为了简单实现渲染）
      class Compile{
        constructor(vm, el, data){
          this.vm = vm
          this.el = el
          this.data = data
          // childNodes可以用来获取dom下的所有子节点
          let nodes = document.querySelector(el).childNodes
          this.compileNodes(nodes)
        }
        compileNodes(nodes){
          for (const node of nodes) {
            // 判断节点的类型，1为标签类型，3为文本类型
            switch (node.nodeType) {
              case 1:
                this.compileNode(node)
                break;

              case 3:
                this.compileText(node)
                break;

              default:
                break;
            }
            // 这里判断是否有标签嵌套情况，然后进行递归
            node.childNodes && node.childNodes.length && this.compileNodes(node.childNodes)
          }
        }
        // 对于文本类型，我们需要解析内部的属性，用来填充内部的文本
        compileText(node){
          let reg = /\{\{(.*)\}\}/
          if(node.textContent !== '' && reg.test(node.textContent)){
            // 获取文本绑定的data属性的key
            let key = reg.exec(node.textContent)[1]
            node.textContent = this.vm[key]
            // new Watcher用来进行依赖的收集，当我绑定的这个值发生改变的时候再执行相应的函数
            new Watcher(this.vm, key, (val) => {
              node.textContent = val
            })
          }
        }
        // 对于标签类型，我们需要解析attr属性用来获取标签上监听了什么事件
        compileNode(node){
          let attrs = node.attributes
          for (const attr of attrs) {
            if(attr.name.indexOf('@') > -1){
              let event = attr.name.split('@')[1]
              let key = attr.value
              node.addEventListener(event, () => {
                this.vm[key]()
              })
            }
            if(attr.name.indexOf('v-model') > -1){
              let key = attr.value
              node.value = this.vm[key]
              node.addEventListener('input', (event) => {
                this.vm[key] = event.target.value
              })
              new Watcher(this.vm, key, (val) => {
                node.value = this.vm[key]
              })
            }
          }
        }

      }
      class Vue{
        constructor({el, data, methods, watch, created, mounted}){
          this.el = el
          this.data = data()
          this.methods = methods
          this.watch = watch
          // 用来代理vue的各种属性
          this.proxy()
          // 进行数据劫持
          new Observer(this.data)
          created && created.call(this)
          // 初始化watch
          this.initWatch()
          // 模板解析
          new Compile(this, el, this.data)
          mounted && mounted.call(this)
        }
        initWatch(){
          for (const key in this.watch) {
            console.log(this.watch[key], 'wawawa')
            new Watcher(this, key, this.watch[key])
          }
        }
        // 代理vue，用来实现this.title替代this.data.title的效果
        proxy(){
          for (const key in this.data) {
            Object.defineProperty(this, key, {
              enumerable:false,
              configurable:true,
              get(){
                // console.log('get');
                return this.data[key]
              },
              set(val){
                // console.log(val, '监听到数据发生了变化');
                this.data[key] = val
              }
            })
          }
          for (const key in this.methods) {
            Object.defineProperty(this, key, {
              enumerable:false,
              configurable:true,
              get(){
                // console.log('get');
                return this.methods[key]
              },
              set(val){
                // console.log(val, '监听到数据发生了变化');
                this.methods[key] = val
              }
            })
          }
        }
      }
      var vm = new Vue({
        el: '#app',
        data(){
          return {
            msg: 'hello vue',
            msg2: 'hello vue2',
            num: 1
          }
        },
        methods: {
          change(){
            // console.log(e)
            console.log('click')
            this.num++
            this.msg = 'hello one'
          }
        },
        watch: {
          num(val, oldVal){
            console.log(val,'watch')
          }
        },
        created(){
          console.log('created')
        },
        mounted(){
          console.log('mounted')
        }
      })
      vm.num = 444
    </script>
  </body>
</html>
```

## 源码推荐阅读

[Vue技术内幕](http://hcysun.me/vue-design/)



## 学习源码能收获什么

1. `带着问题去看源码`，在真实的开发阶段使代码提高多少简洁度和代码可改善度

2. 在看源码的当中，学到了什么，对于一些数据处理，一些代码的小技巧，对于自己开发学可以有哪些`借鉴之处`。

3. `整体的代码思路`，和`逻辑化分`，`代码的组织`能力又有那里值得我们去学习。

4. 从中我们可以拓展出那些其它api的关联和了解，深入对其它api的理解和运用。