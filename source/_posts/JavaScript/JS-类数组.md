---
title: JS-类数组
date: 2019-08-27 10:47:47
categories:
- JavaScript
comments: false
---

### 定义

当我们使用document.querySelectorAll('p') （选择指定的css选择器），返回的就是一个类数组

它是一个含使用从0开始，且自然递增的整数做键名，并且定义了length表示元素个数的对象

<!-- more -->

```js
var array = ['zhangsan', 'lisi', 'zhaoliu'];
var arrayLike = {
    0: 'zhangsan',
    1: 'lisi',
    2: 'zhaoliu',
    length: 3
}
```

### 与数组的相同之处
在数值和长度的读取，自身遍历等方法的用法和数组 相似

### 与数组的不同之处
不存在数组上的操作方法，比如push // arrayLike.push is not a function

### 如何使用数组的方法
通过Function.call Funciton.apply 方法改变this指向

```js
var arrayLike = {
    0: 'name',
    1: 'age',
    2: 'sex',
    length: 3
}
Array.prototype.join.call(arrayLike, ':'); // name:age:sex
Array.prototype.map.call(arrayLike, function(item){
    return  `${item}-map`;
});
// ["name-map", "age-map", "sex-map"]
```

### 怎么 转换成数组
#### slice
```js
Array.prototype.slice.call(arrayLike); // ["name", "age", "sex"] 
```

#### splice
```js
Array.prototype.splice.call(arrayLike, 0); // ["name", "age", "sex"] 
```

#### concat
```js
Array.prototype.concat.apply([], arrayLike)
```

#### ES6 Array.from
```js
Array.from(arrayLike); // ["name", "age", "sex"] 
```

#### ES6 ...运算符 

作为函数参数的时候可以把arguments转换成数组

```js
function translateArray(...arguments) {
    // ...
} 
```

