---
title: Global 类型中不存在 Property
date: 2020-04-28 18:21:47
categories:
- TypeScript
comments: false
---

**问题描述：**为了能够执行日志打印命令，使用 `global.log.info('测试')` ，却始终报错：

```bash
Property 'window' does not exist on type 'Global'.
```

<!-- more -->

**解决办法：**

1. 把以下代码放在脚本的顶部

   ```js
   const globalAny:any = global;
   ```
2. 脚本中使用 `globalAny` 代替 `global`

   ```js
   globalAny.log.info('测试');
   ```

3. 需要在脚本的最后加上以下代码，避免和全局作用域的变量起冲突

   ```js
   export {};
   ```

   