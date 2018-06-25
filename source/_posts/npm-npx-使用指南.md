---
title: npm npx 使用指南
categories:
  - 技术
date: 2018-04-27 16:58:12
tags:
	- npm
	- npx
---

# npm npx 使用指南

## npm

### 原理

npm 脚本的原理非常简单。每当执行 `npm run`，就会自动新建一个 Shell，在这个 Shell 里面执行指定的脚本命令。因此，只要是 Shell（一般是 Bash）可以运行的命令，就可以写在 npm 脚本里面。

比较特别的是，`npm run` 新建的这个 Shell，会将当前目录的 `node_modules/.bin` 子目录加入 `PATH` 变量，执行结束后，再将PATH变量恢复原样。

这意味着，当前目录的 `node_modules/.bin` 子目录里面的所有脚本，都可以直接用脚本名调用，而不必加上路径。比如，当前项目的依赖里面有 Mocha，只要直接写 `mocha test` 就可以了。

``` bash
"test": "mocha test"
```

而不用写成下面这样。


``` bash
"test": "./node_modules/.bin/mocha test"
```

### 执行顺序

如果是并行执行（即同时的平行执行），可以使用 `&` 符号。

``` bash
npm run script1.js & npm run script2.js
```

如果是继发执行（即只有前一个任务成功，才执行下一个任务），可以使用 `&&` 符号。

``` bash
npm run script1.js && npm run script2.js
```

### 钩子

npm 脚本有`pre`和`post`两个钩子。举例来说，`build`脚本命令的钩子就是`prebuild`和`postbuild`。

``` json
"prebuild": "echo I run before the build script",
"build": "cross-env NODE_ENV=production webpack",
"postbuild": "echo I run after the build script"
```

用户执行`npm run build`的时候，会自动按照下面的顺序执行。

``` bash
npm run prebuild && npm run build && npm run postbuild
```

因此，可以在这两个钩子里面，完成一些准备工作和清理工作。下面是一个例子。

``` json
"clean": "rimraf ./dist && mkdir dist",
"prebuild": "npm run clean",
"build": "cross-env NODE_ENV=production webpack"
```

### 常用脚本示例
``` json
// 删除目录
"clean": "rimraf dist/*",

// 本地搭建一个 HTTP 服务
"serve": "http-server -p 9090 dist/",

// 打开浏览器
"open:dev": "opener http://localhost:9090",

// 实时刷新
 "livereload": "live-reload --port 9091 dist/",

// 构建 HTML 文件
"build:html": "jade index.jade > dist/index.html",

// 只要 CSS 文件有变动，就重新执行构建
"watch:css": "watch 'npm run build:css' assets/styles/",

// 只要 HTML 文件有变动，就重新执行构建
"watch:html": "watch 'npm run build:html' assets/html",

// 部署到 Amazon S3
"deploy:prod": "s3-cli sync ./dist/ s3://example-com/prod-site/",

// 构建 favicon
"build:favicon": "node scripts/favicon.js",
```

## npx

### 执行依赖包里的二进制文件

举例来说，之前我们可能会写这样的命令：

``` bash
npm i -D webpack
./node_modules/.bin/webpack -v
```

有了 npx，你只需要这样

``` bash
npm i -D webpack
npx webpack -v
```

npx 会自动查找当前依赖包中的可执行文件，如果找不到，就会去 PATH 里找。如果依然找不到，就会帮你临时安装(one-off commands)

这条命令会临时安装 `create-react-app` 包，命令完成后 `create-react-app` 会删掉，不会出现在 global 中。下次再执行，还是会重新临时安装。

``` bash
$ npx create-react-app my-cool-new-app
```

再比如 npx http-server 可以一句话帮你开启一个静态服务器
``` bash
$ npx create-react-app my-cool-new-app
```




### 运行远程仓库的可执行文件

``` bash
npx github:piuccio/cowsay hello
```

### 运行不同Node.js版本的命令

``` bash
npx node@6 -v
```



> 参考链接
> [阮一峰的网络日志>npm scripts 使用指南](http://www.ruanyifeng.com/blog/2016/10/npm_scripts.html)
>
> [介绍npx：一个npm包执行器](https://blog.csdn.net/whh181/article/details/78363544)
> [npx 是什么](https://zhuanlan.zhihu.com/p/27840803)
