---
title: Vue2.0组件之间通信
date: 2017-09-09 15:15:47
categories:
- Vue
tags:
- Vue-传值
comments: false
---

```
App.vue
<template>
  <div id="app">
  </div>
</template>
<script>
export default {
  name: 'app',
}
</script>
<style>
</style>
```

### 一 .我们先来创建中央事件总线，在src/assets/下创建一个eventBus.js,内容如下

```
import Vue from 'Vue'

export default new Vue
```

eventBus中我们只创建了一个新的Vue实例，以后它就承担起了组件之间通信的桥梁了，也就是中央事件总线。

### 二 . 创建一个firstChild组件，引入eventBus这个事件总线，接着添加一个按钮并绑定一个点击事件

```
<template>
    <div id="firstChild">
        <h2>firstChild组件</h2>
        <button @click="sendMsg">向组件传值</button>
    </div>
</template>

<script>
import bus from '../assets/eventBus'

export default {
    methods:{
        sendMsg:function(){
            bus.$emit("userDefinedEvent","this message is from firstChild")
        }
    }
}
</script>

<style>

</style>
```

我们在响应点击事件的sendMsg函数中用$emit触发了一个自定义的userDefinedEvent事件，并传递了一个字符串参数

PS:$emit实例方法触发当前实例(这里的当前实例就是bus)上的事件,附加参数都会传给监听器回调。

### 三 . 我们再创建一个secondChild组件，引入eventBus事件总线，并用一个p标签来显示传递过来的值

```
<template>
    <div id="secondChild">
        <h2>secondChild组件</h2>
        <p>从firstChild接收的字符串参数：{{msg}}</p>
    </div>
</template>

<script>
import bus from '../assets/eventBus'

export default {
    data(){
        return{
            msg:"默认值"
        }
    },
    mounted(){
        var self = this
        bus.$on("userDefinedEvent",function(msg){
            self.msg = msg
        })
    }
}
</script>

<style>

</style>
```

我们在mounted中，监听了userDefinedEvent,并把传递过来的字符串参数传递给了$on监听器的回调函数

PS:
- mounted:是一个Vue生命周期中的钩子函数，简单点说就类似于jquery的ready，Vue会在文档加载完毕后调用mounted函数。
- $on:监听当前实例上的自定义事件(此处当前实例为bus)。事件可以由$emit触发，回调函数会接收所有传入事件触发函数($emit)的额外参数。

### 四 . 在父组件中，注册这两个组件，并添加这两个组件的标签

```
App.vue
<template>
  <div id="app">
    <firstChild></firstChild>
    <secondChild></secondChild>
  </div>
</template>
<script>
import firstChild from './components/firstChild'
import secondChild from './components/secondChild'
export default {
  name: 'app',
  components:{
    firstChild,
    secondChild
  }
}
</script>
<style>
</style>
```

保存所有修改的文件，然后打开浏览器窗口，内容如下(css请自行处理)


点击向组件传值按钮，我们可以看到传值成功


