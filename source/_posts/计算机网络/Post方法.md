---
title:  Post方法
date: 2019-03-07 15:32:37
categories:
- 计算机网络
tags:
- Post
comments: false
---

## 如何**设置传输的数据的格式**

在 Post 方法中，设定 http的头（header)，在里面声明不同的 content-type

## 传输的数据格式

1. application/x-www-form-urlencoded
	- 最常见的方式；浏览器的 原生表单，如果不设置 enctype 属性，那么最终就会以 application/x-www-form-urlencoded方式提交数据
2. multipart/form-data
	- 使用表单上传文件时
3. application/json
4. text/xml

## 前端向后端传输数据的方法有哪些
1. Ajax
	- 传给后台的数据通过 Json 封装起来，再用 Ajax 将 Json 传到后台，需要添加 Jquery 库
2. 通过 form 表单的 action 传值
3. Vue 的 Axios
4. 通过 Url 字符串拼接传值
   - 直接在 Ajax 中 Url 拼接数据
   - 通过 window.location.href 指定路径提交数据
   - 通过 a 标签的 href 属性提交数据

## 表单是怎么传输数据的
当前端，把前端层面的表单提交时候，会把表单数据，写到 html header里面，然后传送给服务器

服务器从 Request.Form中，拿到数据 -> 数据通过 url解码为表单元素数据 -> 构成一个对象后，再进行处理

