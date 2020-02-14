---
title: router-link 改变默认 a 标签
date: 2020-02-14 16:02:47
categories:
- Vue
tags:
- Vue-Router
comments: false
---



```vue
<router-link tag="div" :to="itemChild.path"><span>{{itemChild.name}}</span></router-link>
```

把 tag 改成 div