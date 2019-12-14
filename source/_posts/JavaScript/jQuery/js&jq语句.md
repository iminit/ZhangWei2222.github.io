---
title:  js和jq语句
date: 2019-04-02 15:32:37
categories:
- JavaScript
tags:
- jQuery
comments: false
---

|      | JQ                                                           | JS                                                           |
| ---- | ------------------------------------------------------------ | :----------------------------------------------------------- |
| 获取 | $(#h1)$(.h1)$('.classTag1')[0].tagName;                      | document.getElementById()document.getElementsByClassName()document.getElementsByTagName() |
| 创建 | var h1 = $('<h1></h1>')                                      | var h1 = document.createElement('<h1></h1>')                 |
| 添加 | before() insertBefore() 插入前面<br />append() appendTo() 插入尾部<br />prepend() prependTo() 插入父元素的最前面<br />after() insertAfter() 插入后面 | document.insertBefore(h1) 头部添加document.appendChild(h1) 尾部添加 |
| 移除 | remove()<br />html()<br />empty()                            | document.removeChild(h1) 删除子元素h1<br />.remove() 删除本身 |
| 复制 | clone()                                                      | var h = h1.cloneNode()                                       |
| 查找 | find() 查找子级 <br />children()查找子级们<br />siblings()查找同级 <br />contents()查找文本节点<br />prev()查找上一级节点 <br />prevAll()查找上一级节点们<br />next()查找下一级节点 <br />nextAll()查找下一级节点们<br />first()第一个节点 <br />last()最后一个节点<br />伪类查找<br />$('xxx'):first 查找第一个节点 <br /> $('xxx'):last 查找最后一个节点<br />$('xxx'):eq(‘下标’)查找指定节点$('xxx'):not('.xxx')查找不含有同级节点  $('xxx'):has('.xxx')查找含有该节点的元素$('xxx'):gt('下标')查找大于指定下标节点  $('xxx'):lt(‘下标’)查找小于指定下标节点$('xxx'):odd查找奇数倍节点       <br />$('xxx'):even 查找偶数倍节点 | childNodes 元素节点和文本节点<br />children 元素节点 <br />firstChild 获取第一个节点（文本节点也算）<br />firstElementChild 文本节点不算，推荐 lastChild <br />lastElementChild获取最后一个节点<br />previousSibling previousElementChild 获取上一个节点<br />nextSibling nextElementSibling 获取下一个节点siblings 获取同级节点 |
