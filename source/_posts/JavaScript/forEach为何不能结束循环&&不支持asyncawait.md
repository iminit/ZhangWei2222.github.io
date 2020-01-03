---
 title: forEach为何不能结束循环&&不支持async/await
date: 2019-12-31 14:32:37
categories:
- JavaScript
tags:
comments: false
---



## 前言

很好奇为啥 forEach 不能结束循环 && 不支持 async/await，今天去找了找原因

<!-- more -->



## 问题

```js
function test() {
  let arr = [3, 2, 1]
  arr.forEach(async item => {
    const res = await fetch(item)
    console.log(res)
  })
  console.log('end')
}

function fetch(x) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve(x)
    }, 500 * x)
  })
}
test()
```

期望值： 3 2 1，实际上： 1 2 3



## 原理

为什么呢？以下是 forEach 的源码

```js
// Production steps of ECMA-262, Edition 5, 15.4.4.18
// Reference: http://es5.github.io/#x15.4.4.18
if (!Array.prototype.forEach) {

  Array.prototype.forEach = function(callback/*, thisArg*/) {

    var T, k;

    if (this == null) {
      throw new TypeError('this is null or not defined');
    }

    // 1. Let O be the result of calling toObject() passing the
    // |this| value as the argument.
    var O = Object(this);

    // 2. Let lenValue be the result of calling the Get() internal
    // method of O with the argument "length".
    // 3. Let len be toUint32(lenValue).
    var len = O.length >>> 0;

    // 4. If isCallable(callback) is false, throw a TypeError exception. 
    // See: http://es5.github.com/#x9.11
    if (typeof callback !== 'function') {
      throw new TypeError(callback + ' is not a function');
    }

    // 5. If thisArg was supplied, let T be thisArg; else let
    // T be undefined.
    if (arguments.length > 1) {
      T = arguments[1];
    }

    // 6. Let k be 0.
    k = 0;

    // 7. Repeat while k < len.
    while (k < len) {

      var kValue;

      // a. Let Pk be ToString(k).
      //    This is implicit for LHS operands of the in operator.
      // b. Let kPresent be the result of calling the HasProperty
      //    internal method of O with argument Pk.
      //    This step can be combined with c.
      // c. If kPresent is true, then
      if (k in O) {

        // i. Let kValue be the result of calling the Get internal
        // method of O with argument Pk.
        kValue = O[k];

        // ii. Call the Call internal method of callback with T as
        // the this value and argument list containing kValue, k, and O.
        callback.call(T, kValue, k, O);
      }
      // d. Increase k by 1.
      k++;
    }
    // 8. return undefined.
  };
}
```

主要看这里

```js
/* 
O 为传入数组
len 为传入数组长度
callback 为传入回调函数
*/
while (k < len) {
  var kValue; 
  if (k in O) { 
    kValue = O[k]; 
    callback.call(T, kValue, k, O);
  } 
  k++;
}
```

可以看到 `callback` 是我们传入的一个被 async 封装的 promise 对象，而 Array.prototype.forEach 内部并没有对这个 promise 对象做任何处理；而且在 `callback` 使用 break也不能结束循环。 

尝试改造一下，让它支持 async, awiat：

```js
Array.prototype.forEach = async function(callback/*, thisArg*/) {

  // ………
  await callback.call(T, kValue, k, O);
  // ………

};
```

所以面对 async await的需求或者跳出循环，使用 for...of 或者 for循环



#### for...of

为啥 for...of 可以呢？机制不同，forEach 是直接调用回调函数，for...of 是通过迭代器的方式去遍历

```js
async function test() {
  let arr = [3, 2, 1]
  const iterator = arr[Symbol.iterator]()
  let res = iterator.next()
  while (!res.done) {
    const value = res.value
    const res1 = await fetch(value)
    console.log(res1)
    res = iterator.next()
  }
  console.log('end')
}
```

