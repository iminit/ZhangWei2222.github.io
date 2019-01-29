---
title: wow.js & animate.css
date: 2018-03-19 15:15:47
categories:
- 前端技术篇
tags:
- 动画
- 小技巧
comments: false
---

## 如何实现随着滚动条滚动加载动画呢
使用wow.js 和animate.css即可实现随滚动条加载酷炫动画

- 引入js和css


```
<link rel="stylesheet" href="css/animate.css">

<script src="js/wow.min.js"></script>
<script>
new WOW().init();
</script>
```

- 在类名加入 wow 和喜欢的动画效果名字


```
<div class="wow bounceInUp">
Content to Reveal Here
</div>
```

