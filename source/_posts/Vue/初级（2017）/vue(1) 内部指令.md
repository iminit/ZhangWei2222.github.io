---
title: vue(1) 内部指令
date: 2017-08-18 15:15:47
categories:
- Vue
tags:
comments: false
---

#### 1.走起我的vue2.0
##### live-server使用
用npm进行全局安装


```
npm install live-server -g
```

在项目目录中打开


```
live-server
```

#### 2.v-if v-else v-show
v-if 和v-show的区别：
- v-if： 判断是否加载，可以减轻服务器的压力，在需要时加载。
- v-show：调整css dispaly属性，可以使客户端操作更加流畅。

#### 3.v-for
数字排序

```
computed:{
   sortItems:function(){
   return this.items.sort(sortNumber);
   }
}

 function sortNumber(a,b){
           return a-b
 }
```

对象循环输出

```
//数组对象方法排序:
function sortByKey(array,key){
    return array.sort(function(a,b){
      var x=a[key];
      var y=b[key];
      return ((x<y)?-1:((x>y)?1:0));
   });
}

sortStudent:function(){
     return sortByKey(this.students,'age');
}
```

#### 4.v-text & v-html

```
<span>{{ message }}</span>=<span v-text="message"></span>

<span v-html="msgHtml"></span>
```

需要注意的是：在生产环境中动态渲染HTML是非常危险的，因为容易导致XSS攻击。所以只能在可信的内容上使用v-html，永远不要在用户提交和可操作的网页上使用。

#### 5.v-on：绑定事件监听器

```
v-on:  =  @
```

ps:文本框的数字会默认转变成字符串，所以我们需要用parseInt()函数进行整数转换。

#### 6.v-model指令
绑定数据源。就是把数据绑定在特定的表单元素上，可以很容易的实现双向数据绑定。

#### 7.v-bind指令
缩写

```
<!-- 完整语法 -->
<a v-bind:href="url"></a>
<!-- 缩写 -->
<a :href="url"></a>
```

#### 8.v-pre&v-cloak&v-once
##### v-pre

```
<div v-pre>{{message}}</div>
```

这时并不会输出我们的message值，而是直接在网页中显示

##### v-cloak
在vue渲染完指定的整个DOM后才进行显示。它必须和CSS样式一起使用，


```
[v-cloak] {
  display: none;
}

<div v-cloak>
  {{ message }}
</div>
```

##### v-once
在第一次DOM时进行渲染，渲染完成后视为静态内容，跳出以后的渲染过程。