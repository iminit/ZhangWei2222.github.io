---
title: JS-变量声明和函数声明
date: 2019-08-27 10:41:47
categories:
- JavaScript
comments: false
---

### 变量声明提前
var 声明一个变量，这个变量会被提前到整个函数的顶部，但没有被赋值

<!-- more -->

```js
(function(){
    console.log(a);//undefined
    var a ="小钻风";
    console.log(a);//小钻风
}())

(function(){
    var a;
    console.log(a);//声明了但未赋值，所以输出undefined；
    a ="小钻风";
    console.log(a);//上一步赋值了，所以输出小钻风
}())
```

### 函数声明提前
只有函数声明格式的函数才会存在函数声明提前，而函数表达式、构造函数不存在

#### 函数创建的三种写法
- 函数声明：function fun( a ) { console.log(a) }; (存在函数声明提前)
- 函数表达式：var fun = function( a ) { console.log(a) };
- 构造函数：var fun = new Function( "a", console.log(a) );

```js
num()//1
console.log(num)//函数本身
function num(){
    console.log(1);
}
num();//1
console.log(num)//函数本身

//实际上
function num(){
    console.log(1);
}
num()//1
console.log(num)//函数本身
num();//1
console.log(num)//函数本身

--- 函数表达式的情况 ---
num()//报错
console.log(num)//undefined
var num = function (){
    console.log(1);
}
num();//1
console.log(num)//函数本身

//实际上
var num;
num()//报错，这时候没有函数声明
console.log(num)//undefined，因为已经声明了num
num = function (){
    console.log(1);
}
num();//1，有函数了，可以调用了
console.log(num)//函数本身，有函数了

```

### 变量声明提前 函数声明提前顺序
函数声明比变量声明更提前

```js
console.log(a);
var a = "孙悟空";
function a(){ console.log("小钻风"); }

//实际上
function a(){
    console.log("小钻风");
}
var a;//由于上面函数已声明a,相同的变量名声明会被直接忽略
console.log(a);//输出函数本体
a = "孙悟空";
```