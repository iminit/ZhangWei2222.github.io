---
title: Vue + Typescript项目@引入无法找到模块问题
date: 2020-04-28 11:14:47
categories:
- Vue
- TypeScript
comments: false
---

问题描述：在项目中如下引入模块会报错

```typescript
import { userInfoAPI, userImageAPI } from "@/services/userAPI.ts";
```

解决办法：只 webpack 中配置 alias 是不行的，还要在 **tsconfig.json** 文件里面配置

```json
{
    "compilerOptions": {
        "baseUrl": ".",
        "paths": {
            "@/*": ["*","src/*"]
        }
    }
}
```

