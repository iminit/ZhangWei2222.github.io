---
title: 四种方式实现JS异步编程
date: 2018-03-23 15:15:47
categories:
- 前端技术篇
tags:
- javaScript
- 异步编程
comments: false
---

## 要求
使用回调、Promise、generator和asnyc/await四种方式，请求api/data中的数据

为方便理解，首先封装了一个库去实现ajax的get方法，调用方式如下


```
http.get('api/data', callback)
```

但是如果以同步的方式来写，如


```
function getData (url) {
  return http.get(url)
}
const data = getData('api/data');
console.log(data) // undefined
```

结果报错了，这是因为http.get(url)本身是个异步（非阻塞）的过程，这行代码运行的时候并没有马上得到结果，它的结果出现时间与服务器有关，是无法预知的。js会把它放到异步线程然后继续运行下去，下一行打印它自然是undefined。

所以才有了接下来的讨论，采用异步的方式去获取它的结果。

## 回调（callback）

```
function getData (url, callback) {
  http.getData(url, data => {
    if (data.status == 200) {
      // let err = null
      // callback(err, data);

      callback(null, data);

    } else {
      // let err = data
      // callback(err, data);

      callback(data);

    }
  });
}

getData('api/data', (err, data) => {
  if (err) {
    console.log(err)
  } else {
    console.log(data)
  }
})
```

## Promise
Promise对象代表一个未完成、但预计将来会完成的操作，有三种状态：

pending：初始值，不是fulfilled，也不是rejected
resolved(也叫fulfilled）：代表操作成功
rejected：代表操作失败

整个promise的状态只支持两种转换：从pending转变为resolved，或从pending转变为rejected，一旦转化发生就会保持这种状态，不可以再发生变化，状态发生变化后会触发then方法。


```
function getData (url) {
  return new Promise((resolve, reject) => {
    http.get(url, data => {
      if (data.status == 200) {
        resolve(data);
      } else {
        reject(data);
      }
    });
  })
}

getData('api/data').then(data => {
  console.log(data);
}).catch(err => {
  console.log(err);
})
```

拓展：
Promise还有一个Promise.all()方法，当我们需要请求多个接口的数据时，可以这么用


```
function getData (url, callback) {
  http.get(url, data => {
    if (data.status == 200) {
      callback(null, data);
    } else {
      callback(data);
    }
  });
}

const ansycTask = {
  getData('data/api1', (err, data) => {
    if (err) {
      console.log(err)
    } else {
      console.log(data)
      return data
    }
  },
  getData('data/api2', (err, data) => {
    if (err) {
      console.log(err)
    } else {
      console.log(data)
      return data
    }
  },
  getData('data/api3', (err, data) => {
    if (err) {
      console.log(err)
    } else {
      console.log(data)
      return data
    }
  }
}

Promise.all(asyncTask).then(data => {
  console.log(data)
})
```

全部请求得到返回后，会返回一个由3个data组成的数组，并且是按请求顺序排列的。

### generator
相当于Promise的升级，可以手动控制Promise的进行


```
function getData (url) {
  return new Promise((resolve, reject) => {
    http.get(url, data => {
      if (data.status == 200) {
        resolve(data);
      } else {
        reject(data);
      }
    });
  })
}

function *getDataGen (url) {
  yield getData(url);
  yield () => {
    console.log('end')
  }
}

const task = getDataGen('api/data');
task.next(); // 打印data
task next(); // 打印'end'
```

generator里面可以使用yield关键字来表示暂停，它接收一个promise对象，返回promise的结果并且停在此处等待，不是一次性执行完。

### async/await
async/await是ES7新特性，必须配合使用，在function前加上async，然后在需要等待结果的代码前面加上await，函数将在await的那个东西取得结果后再赋值，可以和Promise配合使用。


```
function getDataPromise (url) {
  return new Promise((resolve, reject) => {
    http.get(url, data => {
      if (data.status == 200) {
        resolve(data);
      } else {
        reject(data);
      }
    });
  })
}

async function getData(url) {
  await getDataPromise(url);
}

const data = await getData('data/api');
```
