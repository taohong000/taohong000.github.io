---
title: gulp 基础知识
categories:
  - 前端知识
  - 构建工具
date: 2020-04-03 16:32:24
tags:
  - gulp
  - 基础知识
---

> 参考文档：[gulp 官网](https://www.gulpjs.com.cn/)

## 快速开始
### 安装 gulp，作为开发时依赖项

```
npm install --save-dev gulp
```

### 在根目录创建 gulpfile.js

``` javascript
function defaultTask(cb) {
  // place code for your default task here
  cb();
}

exports.default = defaultTask
```

### 执行gulp 命令

```
gulp
```

## 创建任务（task）

gulp 提供了两个组合方法： series() 和 parallel()，允许将多个独立的任务组合为一个更大的操作。

1. series() 是按顺序执行任务, parallel() 是并发执行任务.
2. 这两个方法都可以接受任意数目的任务（task）函数或已经组合的操作.
3. series() 和 parallel() 可以互相嵌套至任意深度。

举例
``` javascript
const { series, parallel } = require('gulp');

function clean(cb) {
  // body omitted
  cb();
}

function css(cb) {
  // body omitted
  cb();
}

function javascript(cb) {
  // body omitted
  cb();
}

exports.build = series(clean, parallel(css, javascript));
```

## 异步执行
### 使用callback

如果任务（task）不返回任何内容，则必须使用 callback 来指示任务已完成。在如下示例中，callback 将作为唯一一个名为 cb() 的参数传递给你的任务（task）。

``` javascript
function callbackTask(cb) {
  // `cb()` should be called by some async work
  cb();
}

exports.default = callbackTask;
```

你通常会将此 callback 函数传递给另一个 API ，而不是自己调用它。

``` javascript
const fs = require('fs');

function passingCallback(cb) {
  fs.access('gulpfile.js', cb);
}

exports.default = passingCallback;
```

### 使用 async/await

如果不使用前面提供到几种方式，你还可以将任务（task）定义为一个 async 函数，它将利用 promise 对你的任务（task）进行包装。这将允许你使用 `await` 处理 promise，并使用其他同步代码。

``` javascript
const fs = require('fs');

async function asyncAwaitTask() {
  const { version } = fs.readFileSync('package.json');
  console.log(version);
  await Promise.resolve('some result');
}

exports.default = asyncAwaitTask;
```

## 处理文件
gulp 暴露了 src() 和 dest() 方法用于处理计算机上存放的文件。

1. src() 接受 [glob](https://www.gulpjs.com.cn/docs/getting-started/explaining-globs/) 参数，并从文件系统中读取文件然后生成一个 Node 流（stream）。 
2. 流（stream）所提供的主要的 API 是 .pipe() 方法, 大多数情况下，利用 .pipe() 方法将插件放置在 src() 和 dest() 之间，并转换流（stream）中的文件。
3. dest() 接受一个输出目录作为参数，并且它还会产生一个 Node 流（stream）。它会将文件内容及文件属性写入到指定的目录中。

``` javascript
const { src, dest } = require('gulp');
const babel = require('gulp-babel');

exports.default = function() {
  return src('src/*.js')
    .pipe(babel())
    .pipe(dest('output/'));
}
```

## 使用插件
Gulp 插件实质上是 Node 转换流（Transform Streams），它封装了通过管道（pipeline）转换文件的常见功能，通常是使用 .pipe() 方法并放在 src() 和 dest() 之间。他们可以更改经过流（stream）的每个文件的文件名、元数据或文件内容。

``` javascript
const { src, dest } = require('gulp');
const uglify = require('gulp-uglify');
const rename = require('gulp-rename');

exports.default = function() {
  return src('src/*.js')
    // gulp-uglify 插件并不改变文件名
    .pipe(uglify())
    // 因此使用 gulp-rename 插件修改文件的扩展名
    .pipe(rename({ extname: '.min.js' }))
    .pipe(dest('output/'));
}
```

### 条件插件
因为插件的操作不应该针对特定文件类型，因此你可能需要使用像 gulp-if 之类的插件来完成转换某些文件的操作。

``` javascript
const { src, dest } = require('gulp');
const gulpif = require('gulp-if');
const uglify = require('gulp-uglify');

function isJavaScript(file) {
  // 判断文件的扩展名是否是 '.js'
  return file.extname === '.js';
}

exports.default = function() {
  // 在同一个管道（pipeline）上处理 JavaScript 和 CSS 文件
  return src(['src/*.js', 'src/*.css'])
    // 只对 JavaScript 文件应用 gulp-uglify 插件
    .pipe(gulpif(isJavaScript, uglify()))
    .pipe(dest('output/'));
}
```

## 文件监控
watch() 将 globs 与 任务（task） 进行关联。它对匹配 glob 的文件进行监控，如果有文件被修改了就执行关联的任务（task）。

``` javascript
const { watch, series } = require('gulp');

function clean(cb) {
  // body omitted
  cb();
}

function javascript(cb) {
  // body omitted
  cb();
}

function css(cb) {
  // body omitted
  cb();
}

// 可以只关联一个任务
watch('src/*.css', css);
// 或者关联一个任务组合
watch('src/*.js', series(clean, javascript));
```