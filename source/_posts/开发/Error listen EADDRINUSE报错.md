---
title: Error listen EADDRINUSE报错
date: 2020-04-28 10:59:48
categories:
- 开发
comments: false
---

报错原因：端口被其他应用程序占用，只要找到占用该端口的应用程序，杀死即可

```bash
# windows
netstat -ano | findstr {端口号}
taskkill /f /t /im {PID号}

# linux
sudo lsof -i:{端口号}
sudo kill -9 {PID号}
```

