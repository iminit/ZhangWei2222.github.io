---
title: eslint报错：invalid-first-character-of-tag-name
date: 2020-04-28 11:07:48
categories:
- 开发
comments: false
---

报错原因：html 特殊字符如**‘大于’ ‘小于’** 号的写法要用**原始码**，如 ‘<’ 原始码为 `&lt;`，‘>’ 原始码为`&gt;`

```html
// 原始错误代码
<span class="site">当前位置 < </span>
    
// 修改后
<span class="site">当前位置 &lt; </span>
```

