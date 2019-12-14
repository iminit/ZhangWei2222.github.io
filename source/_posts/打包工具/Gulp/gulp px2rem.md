---
title: gulp px2rem
date: 2018-03-19 15:15:47
categories:
- 打包工具
tags:
- Gulp
comments: false
---

## 如何用px2rem处理css文件呢？
先把.styl文件编译成.css文件，接着处理


```
//px2rem处理
gulp.task('px2rem',function() {
  return gulp.src('./src/css/index.css')
    .pipe(px2rem({
      baseDpr: 2,             // base device pixel ratio (default: 2)
      threeVersion: false,    // whether to generate @1x, @2x and @3x version (default: false)
      remVersion: true,       // whether to generate rem version (default: true)
      remUnit: 75,            // rem unit value (default: 75)
      remPrecision: 6         // rem precision (default: 6)
    }))
    .pipe(gulp.dest('./src/css'))
});
```

在./src/css中就会生成index.debug.css文件

最终的结果是


```
.selector {
    width: 150px;
    height: 64px; /*px*/
    font-size: 28px; /*px*/
    border: 1px solid #ddd; /*no*/
}

.selector {
    width: 2rem;
    border: 1px solid #ddd;
}
[data-dpr="1"] .selector {
    height: 32px;
    font-size: 14px;
}
[data-dpr="2"] .selector {
    height: 64px;
    font-size: 28px;
}
[data-dpr="3"] .selector {
    height: 96px;
    font-size: 42px;
}
```
