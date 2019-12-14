---
title: split,formatDate,store,util组件
date: 2017-09-09 15:15:47
categories:
- Vue
tags:
- 饿了么教程
comments: false
---

### split组件(vue)的代码
split组件因为常使用,所以单独独立出来,并且结构相当简单.


```
<template>
  <div class="split"></div>
</template>

<script>
  export default {};
</script>

<style lang="stylus" rel="stylesheet/stylus">
  .split
    width: 100%
    height: 16px
    border-top: 1px solid rgba(7, 17, 27, 0.1)
    border-bottom: 1px solid rgba(7, 17, 27, 0.1)
    background: #f3f5f7
</style>
```

### formatDate.js组件
<!--vue过滤器使用-->

```
<div class="time">{{rating.rateTime | formatDate}}</div>
//在es6下,export 函数function的导入需要这样写
import { formatDate } from '../../common/js/date'; //导入自定义的date模块

//vue里面的filters
filters: {
      formatDate(time) {
        let date = new Date(time);
        //调用date模块的formatDate函数来解析时间
        return formatDate(date, 'yyyy-MM-dd hh:mm');
      }
    },
```

formatDate.js是一个自定义的js组件,不是vue组件,目录位于:src/common/js,这种写法是为了练习js的模块化编程

- 将单独的一个函数写成一个模块
- 通过export导出函数
- 通过import导入函数


```
export function formatDate(date, fmt) { //在es6下导出一个函数
//对一个或多个y进行匹配,匹配到就进行年的替换(年有四位,所以需要特殊处理)
  if (/(y+)/.test(fmt)) {
    fmt = fmt.replace(RegExp.$1, (date.getFullYear() + '').substr(4 - RegExp.$1.length));
  }
  let o = {
    'M+': date.getMonth() + 1, //js的月是从0开始算,所以要加1
    'd+': date.getDate(),
    'h+': date.getHours(),
    'm+': date.getMinutes(),
    's+': date.getSeconds()
  };
  //对月,日,时,分,秒进行匹配替换(这些都是两位,可以一起处理)
  for (let k in o) {
    if (new RegExp(`(${k})`).test(fmt)) { //匹配到key例如MM
      let str = o[k] + ''; //然后o['MM'] 就是date.getMonth() + 1

      //如果匹配到的时间是1位数,例如是M,那么就直接使用date.getMonth() + 1的值,
      //如果是两位数,那么就在前面补0,使用padLeftZero函数
      fmt = fmt.replace(RegExp.$1, (RegExp.$1.length === 1) ? str : padLeftZero(str)); 
    }
  }
  return fmt;
};

//先加两个0,然后再根据长度截取(因为最长也就2个0的长度)
function padLeftZero(str) {
  return ('00' + str).substr(str.length);
}
```

### store.js组件
这是一个js模块,负责html5的localstoage存储和读取的,位置: src/common/js/store.js

使用的方法是:


```
//在es6下,export 函数function的导入需要这样写
 import { saveToLocal, loadFromLocal } from '../../common/js/store';
```

代码:


```
//存储
//传入三个参数,seller的id,要存储的key和value
export function saveToLocal (id, key, value) {
//需要加上window对象来使用localstorage
  let seller = window.localStorage.__seller__; //使用__只是一种标记写法,标记是自定义的某种编码规范,这里代表这只是seller的数据
  if (!seller) { //第一次生成seller的时候初始化
    seller = {}; 
    seller[id] = {};
  } else {
    seller = JSON.parse(seller); //json字符串需要解析
    if (!seller[id]) { //不同seller的时候初始化
      seller[id] = {};
    }
  }
  seller[id][key] = value; //生成当前的seller对象
  //localStorage只能存储字符串,需要转成json字符串
  window.localStorage.__seller__ = JSON.stringify(seller);
}

//读取
三个参数,seller的id,之前存储的key,和一个默认值
export function loadFromLocal (id, key, def) {
  let seller = window.localStorage.__seller__;
  if (!seller) { //读取不到返回默认值
    return def; 
  }
  seller = JSON.parse(seller)[id]; //json解析
  if (!seller) { //解析失败返回默认值
    return def;
  }
  let ret = seller[key]; 
  return ret || def; //解析成功但是没有这个seller的id的也返回默认值
}
```

- 在node里面,没有默认全局window对象,所以需要指定加上才能使用window的相关方法和属性
- seller[id][key] = value; 相当于是某个id的seller的某个属性(key)和值(value)保存为一个对象
- 关于写入的逻辑:先读取localstorage的已有值,判断是否存在,然后再去解析localstoage的已有值,判断是否等于当前的数据的key值(id),最后再处理最终的值是否存储,这里逻辑需要先判断已有值.
- 关于读取的逻辑:先读取localstorage判断是否有值,然后再去判断解析localstoage读取得到的值,最后再处理最终得到的值是否正常,按顺序进行逻辑处理
### util.js组件
这个js模块负责获取url的参数,位置:src/common/js/util.js


```
/**
 * 解析url参数
 * @example ?id=12345&a=b
 * @return Object {id:12345,a:b}
 */

export function urlParse () {
  let url = window.location.search; //获取到url的所有参数
  let obj = {};
  let reg = /[?&][^?&]+=[^?&]+/g; //正则判断获取
  let arr = url.match(reg); //正则获取后会保存到一个数组
  // ['?id=12345','&a=b']
  if (arr) {
    arr.forEach((item) => {
      let tempArr = item.substring(1).split('='); //将第一位去掉,然后用等号分隔
      let key = decodeURIComponent(tempArr[0]); //URI对于utf8格式会转码,所以这里需要解码
      let val = decodeURIComponent(tempArr[1]);
      obj[key] = val;
    });
  }
  return obj;
}
```

- 读取url的所有参数,例如http://a.com/?id=12345&a=b的?id=12345&a=b
- 然后进行正则匹配,/[?&][^?&]+=[&?&]+/g,以?id=12345&a=b为举例:
    - [?&] 先匹配?和&的,url参数都是有这个2个字符作为连接符,就是指匹配?
    - [^?&]+ 然后匹配非?和&的多个,就是指匹配id
    
    - = 匹配等号
    - [^?&]+然后匹配非?和&的多个,就是指匹配12345

加起来就是能够匹配?id=12345&a=b
- 用对象返回,方便处理.
