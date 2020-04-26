---
title: switch分支合并
date: 2020-04-26 11:53:47
categories:
- JavaScript
comments: false
---

 switch 分支合并，会 case 穿透：

```js
case 1: case 2:
    key = 1;
    break;
```

