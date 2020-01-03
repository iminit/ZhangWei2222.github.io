---
title: javascript高阶
date: 2019-11-18 11:17:37
categories:
- JavaScript
tags:
comments: false
---



## 类数组 arguments 转化数组？

`arguments`(类数组) 是一个对象，属性从0开始，依次为1，2，3...有`callee`和`length`属性

常见的类数组
- 用`getElementByTagName/ClassName/Name()` 获得的`HTMLCollection`
- 用`querySlector`获得的`nodeList`

### 1、`Array.prototype.slice.call()`

```js
function sum(a, b) {
    let args = Array.prototype.slice.call(arguments);
    console.log(args.reduce((sum, cur) => sum + cur));
}
sum(1, 2) //3
```

### 2、`Array.from()`

```js
function sum(a, b) {
    let args = Array.from(arguments);
    console.log(args.reduce((sum, cur) => sum + cur));
}
sum(1, 2) //3
```

### 3、`ES6`展开运算符

```js
function sum(a, b) {
    let args = [...arguments];
    console.log(args.reduce((sum, cur) => sum + cur));
}
sum(1, 2) //3
```

### 4、利用 `concat`+`apply`

```js
function sum(a, b) {
    let args = Array.prototype.concat.apply([], arguments);
    console.log(args.reduce((sum, cur) => sum + cur));
}
sum(1, 2) //3
```

### 5、最原始、粗暴

用`for`循环把类数组的每个属性值放到里面



## 怎么中断`forEach`循环？

`forEach`不能中断循环，可以使用以下两种方法：

1. 使用 `try` 监视代码块，在需要中断的地方抛出异常
2. （官方推荐，替换方法）用`every`或`some`代替

**`every` 与 `some` 区别**

1.  `every`一旦发现有一个不符合条件，就不会继续下去
2.  `some`一旦找到符合条件，就不会继续下去

```js
let arr = [1, 2, 3, 4];
arr.every(item => {
    console.log(item)
    return item > 2;
}) // 1 false

arr.some(item => {
    console.log(item)
    return item > 2;
}) // 1 2 3 true
```



## 判断数组是否包含某个值？

### 1、`array.indexOf`

如果存在，返回数组元素的`下标`，否则返回 `-1`

```js
let arr = [1, 2, 3, 4];
let index = arr.indexOf(3);
console.log(index); //2
```

### 2、`array.includes`

如果存在，返回`true`，否则返回`false`

```js
let arr = [1, 2, 3, 4];
if(arr.includes(3))
    console.log('存在')；
else
    console.log('不存在')；
```

### 3、`array.find`

返回数组中满足条件的`第一个元素的值`，否则返回`undefined`

```js
let arr = [1, 2, 3, 4];
let result = arr.find(item => {
    return item > 3;
})
console.log(result); // 4
```

### 4、`array.findIndex`

返回数组中满足条件的`第一个元素的下标`，否则返回`-1`

```js
let arr = [1, 2, 3, 4];
let result = arr.findIndex(item => {
    return item > 3;
})
console.log(result); // 3
```



## 数组扁平化 多层数组转化为一级数组？

```js
let arr = [1, 2, [3, [4, 5]], 6];
let str = JSON.stringify(arr);
```

### 1、`ES6`的`flat`方法

```js
arr = arr.flat(Infinity); // [1, 2, 3, 4, 5, 6]
```

### 2、`replace`+`split`

```js
arr = str.replace(/(\[}\])/g,'').split(',')
```

### 3、`replace`+`JSON.parse`

```js
str = str.replace(/(\[}\])/g,'');
str = '[' + str + ']';
arr = JSON.parse(str);
```

### 4、普通递归

```js
let result = [];
let fn = function(ary) {
    for(let i = 0;i < ary.length; i++) {
        let item = ary[i];
        if(Array.isArray(ary[i])){
            fn(item);
        }else {
            result.push(item)
        }
    }
}
```

### 5、利用`reduce`函数迭代

```js
function flatten(ary) {
    return ary.reduce((pre, cur) => {
        return pre.concat(Array.isArray(cur) ? flatten(cur) : cur);
    },[]);
}
console.log(flatten(arr));
```

### 6、扩展运算符

```js
while(arr.some(Array.isArray())) {
    arr = [].concat(...arr);
}
```



## 高阶函数

> 一个函数可以接收另一个函数作为参数 或 返回值为一个函数

### 1、map
参数
- 两个参数：回调函数，回调函数的`this`(可选)
    - 回调函数默认传入三个值：依次为 当前元素、当前索引、整个数组

对原来数组没有影响；新建一个数组，数组中的每个元素都是原来数组元素调用回调函数返回的结果

```js
let nums = [1, 2, 3];
let obj = {val: 5};
let newNums = nums.map(function(item, index, array) {
    return item + index + array[index] + this.val;
},obj);
console.log(newNums); // [7, 10, 13]

```

### 2、reduce
参数
- 两个参数：回调函数，初始值
    - 回调函数三个默认参数，依次为 积累值、当前值、整个数组

```js
let nums = [1, 2, 3];
let newNums = nums.reduce(function(preSum, curVal, array) {
    return preSum + curVal;
}, 0);
console.log(newNums); // 6
```

不传默认值会自动以第一个元素为初始值，然后从第二个元素开始依次累计。

### 3、filter
参数
- 一个函数参数
    - 默认参数：当前元素

返回值
- 一个布尔类型，决定元素是否保留

```js
let nums = [1, 2, 3];
let oddNums = nums.filter( item => item % 2);
```

### 4、sort
参数
- 一个用于比较的函数
    - 两个默认参数：比较的两个元素

```js
let nums = [2, 3, 1];
nums.sort(function(a, b) {
    if(a > b) return 1; // a在b后面，a下标比b大
    else if(a < b) return -1;
    else if(a == b) return 0;
})
// [1, 2, 3]
```

如果不传函数，则将数字转换为字符串，根据字母 `unicode值` 进行升序排序



## `JSON.parse()` 与 `JSON.stringfy()`

### `JSON.parse()`

将JavaScript对象表示法的JSON字符串转换为对象(字符串转对象)

```js
var jsonStr = '{"name":"leinov","sex":"famle","address":"beijing"}'
var jsonObj = JSON.parse(jsonStr);
alert(typeof jsonObj); //Object
```

### `JSON.stringfy()`

将 JavaScript json对象转换为JavaScript对象表示法的JSON字符串(对象转为字符串)

```js
var student = new Object();
student.name = "leinov";
student.sex = "famle";
student.address = "chaoyang";
var jsonStudent = JSON.stringify(student);

alert(typeof jsonStudent); //string
```