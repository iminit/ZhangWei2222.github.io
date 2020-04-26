---
title: log4js 配置
date: 2020-04-26 16:54:47
categories:
- 服务器
tags:
comments: false
---

使用 Node.js 编写后端程序，为了调试、debugger代码，当然少不了日志，log4js 是一个很好的日志管理工具，下面记录它的配置
<!-- more -->



### 安装

```bash
npm install log4js --save
```



### 编写配置文件

创建 logs 文件夹，添加 `log_config.json` 文件

```json
{
  "appenders": {
    "console": {
      "type": "console",
      "category": "console"
    },
    "info": {
      "category": "log_info",
      "type": "dateFile",                   // 可以设置成 console、file、dateFile三种
      "filename": "./logs/info/info_file",  // 设置 log 输出的文件路径
      "pattern": "yyyy-MM-dd.log",          // 设置 log 输出的文件名
      "alwaysIncludePattern": true,         // 和上面同时使用 设置每天生成log名
      "backups": 4,                         // 仅保留最新的四个日志文件
      "maxLogSize ": 10000000,              // 设置文件大小
      "encoding": "utf-8"                   // 设置文件编码格式
    },
    "error": {
      "category": "log_error",
      "type": "dateFile",
      "filename": "./logs/error/error_file",
      "pattern": "yyyy-MM-dd.log",
      "alwaysIncludePattern": true,
      "backups": 4,
      "maxLogSize ": 10000000,
      "encoding": "utf-8"
    },
    "minError": {
      "type": "logLevelFilter",
      "appender": "error",
      "level": "error"                      // 设置log输出的最低级别
    },
    "debug": {
      "category": "log_debug",
      "type": "dateFile",
      "filename": "./logs/debug/debug_file",
      "pattern": "yyyy-MM-dd.log",
      "alwaysIncludePattern": true,
      "backups": 4,
      "maxLogSize ": 10000000,
      "encoding": "utf-8"
    },
    "maxDebug": {
      "type": "logLevelFilter",
      "appender": "debug",
      "level": "debug",
      "maxLevel": "debug"                   // 设置log输出的最高级别 
      // log级别为8级 ALL<TRACE<DEBUG<INFO<WARN<ERROR<FATAL<MARK<OFF。默认级别是 OFF
    },
    "trace": {
      "category": "log_trace",
      "type": "dateFile",
      "filename": "./logs/trace/trace_file",
      "pattern": "yyyy-MM-dd.log",
      "alwaysIncludePattern": true,
      "backups": 4,
      "maxLogSize ": 10000000,
      "encoding": "utf-8"
    },
    "maxTrace": {
      "type": "logLevelFilter",
      "appender": "trace",
      "level": "trace",
      "maxLevel": "trace"
    }
  },
  "categories": {
    "default": {
      "appenders": [
        "console",
        "info",
        "minError",
        "maxDebug",
        "maxTrace"
      ],
      "level": "ALL"
    }
  },
  "replaceConsole": true
}
```



### 使用

在后端执行文件中，添加启动日志服务代码

```typescript
const log4js = require("log4js");
const log4js_config = require("../logs/log_config.json");
log4js.configure(log4js_config);
global.log = log4js.getLogger("wei");

// 使用
global.log.info('---开始listen in localhost:' + config.port);
```

随后在 logs 文件夹中，可以看到各分类文件夹及相关文件

如果仅在为了调试，可以使用 `console.log` 进行输出数据查看

<img src="log4js 配置.assets/image-20200426171054080.png" alt="image-20200426171054080" style="zoom: 67%;" />