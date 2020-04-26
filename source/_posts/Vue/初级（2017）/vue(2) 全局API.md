---
title: vue(2) 全局API
date: 2017-08-18 15:15:47
categories:
- Vue
tags:
comments: false
---

#### 1.Vue.directive 自定义指令
##### 什么是全局API？
在构造器外部用Vue提供给我们的API函数来定义新的功能。

##### 自定义指令中传递的三个参数
el: 指令所绑定的元素，可以用来直接操作DOM。

binding: 一个对象，包含指令的很多信息。

vnode: Vue编译生成的虚拟节点。

##### 自定义指令的生命周期
自定义指令有五个生命周期（也叫钩子函数），分别是 bind,inserted,update,componentUpdated,unbind

1. bind:只调用一次，指令第一次绑定到元素时调用，用这个钩子函数可以定义一个绑定时执行一次的初始化动作。
1. inserted:被绑定元素插入父节点时调用（父节点存在即可调用，不必存在于document中）。
1. update:被绑定于元素所在的模板更新时调用，而无论绑定值是否变化。通过比较更新前后的绑定值，可以忽略不必要的模板更新。
1. componentUpdated:被绑定元素所在模板完成一次更新周期时调用。
1. unbind:只调用一次，指令与元素解绑时调用。

#### 2.Vue.extend构造器的延伸
##### 什么是Vue.extend？
以简单理解为当在模板中遇到该组件名称作为标签的自定义元素时，会自动调用“扩展实例构造器”来生产组件实例，并挂载到自定义元素上。

#### 3.Vue.set全局操作
就是在构造器外部操作构造器内部的数据、属性或者方法。

##### 引用构造器外部数据
什么是外部数据，就是不在Vue构造器里里的data处声明，而是在构造器外部声明，然后在data处引用就可以了。

##### 为什么要有Vue.set的存在?
由于Javascript的限制，Vue不能自动检测以下变动的数组。

*当你利用索引直接设置一个项时，vue不会为我们自动更新。

*当你修改数组的长度时，vue不会为我们自动更新。

#### 4.Vue的生命周期（钩子函数）


#### 5.Template 制作模版
##### 1.直接写在选项里的模板
直接在构造器里的template选项后边编写。这种写法比较直观，但是如果模板html代码太多，不建议这么写。


```
var app=new Vue({
     el:'#app',
     data:{
         message:'hello Vue!'
      },
     template:`
        <h1 style="color:red">我是选项模板</h1>
     `
})
```

##### 2.写在
这种写法更像是在写HTML代码，就算不会写Vue的人，也可以制作页面


```
<template id="demo2">
         <h2 style="color:red">我是template标签模板</h2>
</template>

<script type="text/javascript">
    var app=new Vue({
        el:'#app',
        data:{
            message:'hello Vue!'
        },
        template:'#demo2'
    })
</script>
```

##### 3.写在标签里的模板
这种写模板的方法，可以让模板文件从外部引入。


```
<script type="x-template" id="demo3">
    <h2 style="color:red">我是script标签模板</h2>
</script>

<script type="text/javascript">
    var app=new Vue({
        el:'#app',
        data:{
            message:'hello Vue!'
        },
        template:'#demo3'
    })
</script>
```

#### 6.Component 初识组件
组件就是制作自定义的标签，这些标签在HTML中是没有的。

##### 1.全局化注册组件
全局化就是在构造器的外部用Vue.component来注册

##### 2.局部注册组件
部注册组件和全局注册组件是向对应的，局部注册的组件只能在组件注册的作用域里进行使用，其他作用域使用无效。

但是你需要注意的是，构造器里的components 是加s的，而全局注册是不加s的。

##### 3.组件和指令的区别
组件注册的是一个标签，而指令注册的是已有标签里的一个属性。

#### 7.Component 组件props 属性设置
props选项就是设置和获取标签上的属性值的

##### 1.定义属性并获取属性值
定义属性我们需要用props选项，加上数组形式的属性名称，例如：props:[‘here’]。在组件的模板里读出属性值只需要用插值的形式，例如.

##### 2.属性中带’-‘的处理方式
小驼峰式写法

##### 3.在构造器里向组件中传值
把构造器中data的值传递给组件，我们只要进行绑定就可以了。

#### 8.Component 父子组件关系
在实际开发中我们经常会遇到在一个自定义组件中要使用其他自定义组件，这就需要一个父子组件关系。

1.构造器外部写局部注册组件
2.父子组件的嵌套

#### 9.Component 标签
标签是Vue框架自定义的标签，它的用途就是可以动态绑定我们的组件，根据数据的不同更换不同的组件。