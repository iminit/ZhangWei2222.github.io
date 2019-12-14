---
title: gulp初接触 
date: 2018-03-15 15:15:47
categories:
- 打包工具
tags:
- Gulp
comments: false
---

## gulp安装

- 全局：npm install gulp -g
- 开发环境：npm install gulp –save-dev

## 项目结构

```
-dist //打包目录
    -css
        -built.css
        -built.min.css
    -js
        -built.js
        -built.min.js
    -images
-src //开发目录
    -css
        -built.css
        -built.css.map
    -js
        -js1.js
        -js2.js
    -stylus
        -stylus1.styl
        -stylus2.styl
    -images
    -index.html //项目入口
-gulpfile.js //配置文件
-package-lock.json
-package.json //配置信息
```

首先在package.json文件中添加项目信息


```
{
    "name": "gulp_test",
    "version": "1.0.0"
}
```

## 引入模块和插件
### 1.引入模块

```
// gulpfile.js
var gulp = require('gulp');
```

### 2.插件
gulp中的几个比较常用的插件
> gulp-concat : 合并文件(js/css)
> 
> gulp-rename : 文件重命名
> 
> gulp-htmlmin : 压缩html文件
> 
> gulp-imagemin : 压缩图片
> 
> gulp-uglify : 压缩js文件
> 
> gulp-livereload : 实时自动编译刷新
> 
> gulp-connect : 热加载
> 
> gulp-stylus : 编译stylus
> 
> gulp-clean-css : 压缩css
> 
> gulp-sourcemaps : 可在浏览器调试工具中查看到对应代码在stylus的位置
> 
> gulp-postcss : 扩展的语法和特性转换成现代的浏览器友好的CSS
> 
> autoprefixer : 自动添加css浏览器前缀
> 
> cssnano : 压缩，去空格，去注释，去覆盖等
> 

API
> gulp.src(filePath/pathArr)：指向指定路径的所有文件，返回文件流对象，用于读取文件
> 
> gulp.dest(dirPath/pathArr)：指向指定的所有文件夹，用于向文件夹中输出文件
> 
> gulp.task(name, [deps], fn)：定义一个任务
> 
> gulp.watch()：监听文件的变化

### 3.各部分打包代码
#### stylus css处理

```
gulp.task('stylusTask', function () {
  var processor = [
    autoprefixer,
    cssnano
  ];
  return gulp.src('src/stylus/*.styl')
    .pipe(sourcemaps.init())
    .pipe(stylus())
    .pipe(postcss(processor))
    .pipe(concat('built.css'))
    .pipe(sourcemaps.write('./'))
    .pipe(gulp.dest('src/css'))
    .pipe(livereload());
});
```

#### 打包asset

```
gulp.task('copyAsset', function () {
  return gulp.src([
    './src/asset/**/*'
    // './src/asset/jquery/**/*'
  ])
    .pipe(gulp.dest('./dist/asset'))
})
```

#### 打包html

```
gulp.task('copyHtml', function () {
  return gulp.src('./src/*.html')
    .pipe(htmlmin({
        collapseWhitespace:true,
        collapseBooleanAttributes:true,
        removeComments:true,
        removeEmptyAttributes:true,
        removeScriptTypeAttributes:true,
        removeStyleLinkTypeAttributes:true,
        minifyJS:true,
        minifyCSS:true    
    }))
    .pipe(gulp.dest('./dist'))
})
```

#### 打包css

```
gulp.task('cssTask', ['stylusTask'], function () {
  return gulp.src('./src/css/built.css')
    .pipe(gulp.dest('./dist/css'))
    .pipe(rename({suffix:'.min'}))
    .pipe(cleanCSS({compatibility:'ie8'}))
    .pipe(gulp.dest('./dist/css'))
})
```

#### 打包js

```
gulp.task('jsTask', function () {
  return gulp.src('./src/js/*.js')
    .pipe(concat('built.js')) //合并到临时文件夹
    .pipe(gulp.dest('./dist/js')) //生成到目标文件夹
    .pipe(rename({suffix:'.min'})) //重命名
    .pipe(uglify()) //压缩
    .pipe(gulp.dest('./dist/js'))
})
```

#### 打包图片

```
gulp.task('copyImages', function () {
  return gulp.src('./src/images/*')
    .pipe(imagemin({
      optimizationLevel: 5, //类型：Number  默认：3  取值范围：0-7（优化等级）
      progressive: true, //类型：Boolean 默认：false 无损压缩jpg图片
      interlaced: true, //类型：Boolean 默认：false 隔行扫描gif进行渲染
      multipass: true //类型：Boolean 默认：false 多次优化svg直到完全优化
    }))
    .pipe(gulp.dest('./dist/images'))
})
```

#### 热加载

```
gulp.task('reload', function () {
  return gulp.src('src/*.html')
    .pipe(connect.reload())
});
监听
gulp.task('server', function () {
  connect.server({
    root: 'src',
    livereload: true,
    port: 5000
  })
});
```

```
gulp.task('auto', function() {
  gulp.watch('src/stylus/*.styl', ['stylusTask']);
  gulp.watch('src/css/*.css', ['reload'])
  gulp.watch('src/*.html', ['reload']);
  gulp.watch('src/js/*.js', ['reload']);
});
```

#### 执行任务

```
gulp.task('default', ['server', 'auto']);

gulp.task('product', ['cssTask', 'copyHtml', 'copyImages', 'jsTask', 'copyAsset', 'copyImages'])
```

#### 完整代码

```
// /*
// * gulp-concat : 合并文件(js/css)
// * gulp-rename : 文件重命名
// * gulp-htmlmin : 压缩html文件
// * gulp-imagemin : 压缩图片
// * gulp-uglify : 压缩js文件
// * gulp-livereload : 实时自动编译刷新
// * gulp-connect : 热加载

// * gulp-stylus : 编译stylus
// * gulp-clean-css : 压缩css
// * gulp-sourcemaps : 可在浏览器调试工具中查看到对应代码在stylus的位置
// * gulp-postcss : 扩展的语法和特性转换成现代的浏览器友好的CSS
// * autoprefixer : 自动添加css浏览器前缀
// * cssnano : 压缩，去空格，去注释，去覆盖等

// *
// * gulp.src(filePath/pathArr)：指向指定路径的所有文件, 返回文件流对象，用于读取文件
// * gulp.dest(dirPath/pathArr)：指向指定的所有文件夹，用于向文件夹中输出文件
// * gulp.task(name, [deps], fn)：定义一个任务
// * gulp.watch()：监视文件的变化
// */

var gulp = require('gulp');

// common plyugin
var concat = require('gulp-concat'),
    rename = require('gulp-rename'),
    htmlmin = require('gulp-htmlmin'),
    imagemin = require('gulp-imagemin'),
    uglify = require('gulp-uglify'),
    livereload = require('gulp-livereload'),
    connect = require('gulp-connect');

var stylus = require('gulp-stylus'),
    sourcemaps = require('gulp-sourcemaps'),
    postcss = require('gulp-postcss'),
    autoprefixer = require('autoprefixer'),
    cleanCSS = require('gulp-clean-css'),
    cssnano = require('cssnano');

// var cheerio = require('gulp-cheerio');

// instruction
gulp.task('help',function () {
  console.log('    gulp                       自定义开发模式');
  console.log('    gulp build                文件打包');
  console.log('    gulp watch                文件监控打包');
  console.log('    gulp help                  gulp参数说明');
  console.log('    gulp server                测试server');
  console.log('    gulp -p                      生产环境（默认生产环境）');
  console.log('    gulp -d                      开发环境');
  console.log('    gulp -m <module>        部分模块打包（默认全部打包）');
});

//stylus css处理
gulp.task('stylusTask', function () {
  var processor = [
    autoprefixer,
    cssnano
  ];
  return gulp.src('src/stylus/*.styl')
    .pipe(sourcemaps.init())
    .pipe(stylus())
    .pipe(postcss(processor))
    .pipe(concat('built.css'))
    .pipe(sourcemaps.write('./'))
    .pipe(gulp.dest('src/css'))
    .pipe(livereload());
});

//打包asset
gulp.task('copyAsset', function () {
  return gulp.src([
    './src/asset/**/*'
    // './src/asset/jquery/**/*'
  ])
    .pipe(gulp.dest('./dist/asset'))
})

//打包html
gulp.task('copyHtml', function () {
  return gulp.src('./src/*.html')
    .pipe(htmlmin({
        collapseWhitespace:true,
        collapseBooleanAttributes:true,
        removeComments:true,
        removeEmptyAttributes:true,
        removeScriptTypeAttributes:true,
        removeStyleLinkTypeAttributes:true,
        minifyJS:true,
        minifyCSS:true    
    }))
    .pipe(gulp.dest('./dist'))
})

//打包css
gulp.task('cssTask', ['stylusTask'], function () {
  return gulp.src('./src/css/built.css')
    .pipe(gulp.dest('./dist/css'))
    .pipe(rename({suffix:'.min'}))
    .pipe(cleanCSS({compatibility:'ie8'}))
    .pipe(gulp.dest('./dist/css'))
})

//打包js
gulp.task('jsTask', function () {
  return gulp.src('./src/js/*.js')
    .pipe(concat('built.js')) //合并到临时文件夹
    .pipe(gulp.dest('./dist/js')) //生成到目标文件夹
    .pipe(rename({suffix:'.min'})) //重命名
    .pipe(uglify()) //压缩
    .pipe(gulp.dest('./dist/js'))
})

//打包图片
gulp.task('copyImages', function () {
  return gulp.src('./src/images/*')
    .pipe(imagemin({
      optimizationLevel: 5, //类型：Number  默认：3  取值范围：0-7（优化等级）
      progressive: true, //类型：Boolean 默认：false 无损压缩jpg图片
      interlaced: true, //类型：Boolean 默认：false 隔行扫描gif进行渲染
      multipass: true //类型：Boolean 默认：false 多次优化svg直到完全优化
    }))
    .pipe(gulp.dest('./dist/images'))
})

//热加载
gulp.task('reload', function () {
  return gulp.src('src/*.html')
    .pipe(connect.reload())
});

//监听
gulp.task('server', function () {
  connect.server({
    root: 'src',
    livereload: true,
    port: 5000
  })
});

gulp.task('auto', function() {
  gulp.watch('src/stylus/*.styl', ['stylusTask']);
  gulp.watch('src/css/*.css', ['reload'])
  gulp.watch('src/*.html', ['reload']);
  gulp.watch('src/js/*.js', ['reload']);
});


//执行任务
gulp.task('default', ['server', 'auto']);

gulp.task('product', ['cssTask', 'copyHtml', 'copyImages', 'jsTask', 'copyAsset', 'copyImages'])
```
