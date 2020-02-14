---
title: prettier 报错
date: 2020-02-14 11:19:48
categories:
- 开发
tags:
- vscode
comments: false
---



> Delete `␍`eslint(prettier/prettier)
>

**解决方法：**

可以修改 `.eslintrc` 文件，在 `rules` 添加一条

```json
"prettier/prettier": "off"
```