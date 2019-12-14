---
title: JS-new实现
date: 2019-09-10 11:49:37
categories:
- JavaScript
tags:
- 原型链
- this
comments: false
---

## 前言

之前我们在 this指向 这篇文章中，讨论过 构造函数 new的步骤：

- 创建一个新对象

- 把构造函数的 this 指向新对象

- 新对象的 proto 属性指向构造函数的 prototype 原型

- 执行构造函数的代码

- 返回新对象

现在，我们尝试依照这个步骤，用 JS 实现一个 new

<!-- more -->



## 实现

```js
function newFn(fn, ...args) {
    let newObj = {}; //创建一个新对象
    const result = fn.call(newObj, ...args); //把构造函数的 this 指向新对象
    newObj._proto_ = fn.prototype; //新对象的 proto 属性指向构造函数的 prototype 原型
    
    if (result && (typeof result === 'object' || typeof result ==='function')) {
        newObj = result
    }

    return newObj //返回新对象
}

//测试用例
function People(name,age){
  this.name = name;
  this.age = age;
}

let people = newFn1(People,'Rose',18);  //调用自定义newFn实现new
console.log(people.name) //Rose
console.log(people2.age) //18
```

