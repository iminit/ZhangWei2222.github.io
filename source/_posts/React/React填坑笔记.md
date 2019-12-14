---
title:  React填坑笔记
date: 2019-07-10 19:58:37
categories:
- React
tags:
- 填坑
comments: false
---

 #### 数组循环时缺少unique "key" prop
 ```javascript
warning:Each child in an array or iterator should have a unipue "key" prop.check the render method.
 ```
注意render里的map，需要加key


#### videoJs报错
```javascript
VIDEOJS:ERROR:The "flash" tech is undefined.skipped browser support check for that tech.
```
删除掉playerOptions的配置属性techOrder即可


#### videoJs与ReactJs冲突报错
当使用ReactJs和videoJs时，将有两个相同data-reactid属性的元素，但ReactJs只允许唯一data-reactid

```javascript
Uncaught Error:Invaeriant Violation:ReactMount Tow valid but unequal nodes with the same 'data-reactid'
```
在video.js包的js文件中，修改：
- 为了避免之后npm i后覆盖已更改的包，重新发布一个自定义的包@meili/videoJs，拷贝原npm包，把package.json的内容改成npm包仓库内的package.json内容，并把script的内容清空，最后更改包名字，npm login npm publish


```javascript
if(attr === 'class') {
    el.className = attrs[attr];
}else if(attr === 'data-reactid') {
    el.setAttribute(attr, attrs[attr] + '_foo');
}else {
    el.setAttribute(attr, attrs[attr]);
}
```