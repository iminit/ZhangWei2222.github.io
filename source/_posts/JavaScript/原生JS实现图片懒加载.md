---
title: 原生JS实现图片懒加载
date: 2019-08-09 16:19:37
categories:
- JavaScript
tags:
- 懒加载
comments: false
---

## 前言
在项目中有一个图片懒加载的需求，因为轮子无法满足老版本的react，所以手动写了一个，顺便对比几种方法

## 实现原理
无论哪种方法，原理都是
- 图片Img标签自定义属性data-src来存放真实的地址
- 当滚动时，检查所有img标签，是否出现在视野范围内与是否已加载，如果出现在视野范围内与未加载，则加载图片。

## 方法1：offsetTop-scroolTop<clientHeight
- element.offsetTop  //获取元素相对于文档顶部的高度
- element.scrollTop  //滚动条滚动的距离
- element.clientHeight  //获取屏幕可视区域的高度

![image](http://s11.mogucdn.com/mlcdn/c45406/190812_5028a5j1j0bi2idfa76bfdgba2g07_720x405.jpg)

offsetTop-scroolTop<clientHeight，则图片进入了可视区内

## 方法2：getBoundingClientRect()
通过getBoundingClientRect()方法来获取元素的大小以及位置，它会返回一个名为ClientRect的DOMRect对象，包含了top、right、botton、left、width、height这些值。

![image](http://s11.mogucdn.com/mlcdn/c45406/190812_635lj70h9h68d78l0c31agii794if_500x500.jpg)

bound.top(获取图片到可视区顶部的距离) <= clientHeight(可视区高度) ，则图片进入了可视区内

## 方法3：IntersectionObserver
新的API，可以自动"观察"元素是否可见，Chrome 51+ 已经支持

#### 注意
IntersectionObserver API 是异步的，不随着目标元素的滚动同步触发。

规格写明，IntersectionObserver的实现，应该采用requestIdleCallback()，即只有线程空闲下来，才会执行观察器。这意味着，这个观察器的优先级非常低，只在其他任务执行完，浏览器有了空闲才会执行。

```javascript
基本用法：
var io = new IntersectionObserver(callback, option);
// 开始观察
io.observe(document.getElementById('example'));
// 停止观察
io.unobserve(element);
// 关闭观察器
io.disconnect();
```
详细用法，参见阮一峰大大：http://www.ruanyifeng.com/blog/2016/11/intersectionobserver_api.html

我们需要用到callback的intersectionRatio属性(目标元素的可见比例)来判断是否在可视区域内，当intersectionRatio > 0 && intersectionRatio <= 1,则图片进入了可视区内

## 代码
```javascript
window.addEventListener('scroll', this.throttle(this.checkScroll));

throttle(fn, mustRun = 300){
  const timer = null;
  let previous = null;
  return function() {
    const now = new Date();
    const context = this;
    const args = arguments;
    if (!previous){
      previous = now;
    }
    const remaining = now - previous;
    if (mustRun && remaining >= mustRun) {
      fn.apply(context, args);
      previous = now;
    }
  }
},

//检查图片是否在可视区内，如果不在，则加载
checkScroll(){
  let scrollImages = document.querySelectorAll('img');
  let self = this;
  Array.from(scrollImages).forEach(function(el){
    if(self.isIn(el) && !self.isLoaded(el)){
    	self.loadImg(el);
    }
  })
},

//方法1：offsetTop-scroolTop<clientHeight，表示图片已进入可视区
isIn(el){
	var H = document.documentElement.clientHeight;//获取可视区域高度
	var S = document.documentElement.scrollTop || document.body.scrollTop;
  return H + S > getTop(el);
}

//offsetTop是元素与offsetParent的距离，循环获取直到页面顶部
getTop(el){
  var T = e.offsetTop;
  while(e = e.offsetParent) {
  	T += e.offsetTop;
  }
  return T;
}
        
//方法2：用来判断bound.top<=clientHeight的函数，返回一个bool值
isIn(el){
  var bound = el.getBoundingClientRect();
  var clientHeight = window.innerHeight;
  return bound.top <= clientHeight;
}, 

//方法3：IntersectionObserver
isIn(el){
  const io = new IntersectionObserver(ioes => {
  let self = this;
  ioes.forEach(ioe => {
    const el = ioe.target;
    const intersectionRatio = ioe.intersectionRatio;
    if (intersectionRatio > 0 && intersectionRatio <= 1) {
    	self.loadImg(el);
    }
    el.onload = el.onerror = () => io.unobserve(el);
    });
  })
  io.observe(el);
},


//判断是否被加载过
isLoaded(el){
  // console.log(el)
  if(el.getAttribute('data-src')) {
  	return el.getAttribute('src')===el.getAttribute('data-src');
  } else {
  	return true;
  }
},

loadImg(el){
  if(!el.src){
    var source = el.dataset.src;
    el.src = source;
  }
}
```