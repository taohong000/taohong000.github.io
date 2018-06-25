---
title: 参考vue-cli的脚手架开发
categories:
  - 技术
date: 2018-05-17 10:18:18
tags:
  - 脚手架
---

## 前言

我在公司的业务主要有三种：
- 管理后台，页面结构基本不变，模块可重复使用。
- webApp，页面的交互较复杂，要根据设计深度定制。
- 简单页面，一般是配合native app 使用的页面，例如活动页面，简单的内容介绍页面，交互少，开发周期短。

业务上基本形成了稳定的套路，这时候开发一套自己的脚手架工具是必要的
- 减少重复性工作，提高开发效率。
- 给不同的业务创建不同的模版，容易迭代和维护。

一方面是为了业务需要，一方面是为了练手，决定仿照vue-cli写一个脚手架工具。

## 思路

vue-cli 整体思路：
- 将脚手架和各个模板独立发布到Git上。
- 通过脚手架下载模版。
- 通过与脚手架的交互信息，渲染模版，得到项目基本结构。

阅读vue-cli的代码，代码思路如下：

1. `vue` 命令会执行 `package.json` 下的 `bin` 中指定的文件，也就是 `bin/vue`。这个文件中主要用到 `commander` 这个包，这个包可以用来设置不同的命令。
2. `vue init` 执行 `bin/vue-init`。
3. `vue init` 根据你输入的官方模版或者远程仓库中的模板名下载模版，用到的包是 `download-git-repo`。
4. 仓库中有 `template` 目录，这里面是模版，有 `meta.js` 或者 `meta.json` 文件，里面的是配置内容，会被 `inquirer` 这个包使用，进行交互式问答，后面根据问答结果渲染模版。
5. `metalsmith` 这个包根据问答内容渲染模版，这里还用到了模版引擎 `handlers`。

## 远程仓库工程结构
```
/template # ------ 模版
meta.json # ------ 互动内容
```

## 脚手架工程结构

```
/bin  # ------ 命令执行文件
/lib  # ------ 工具模块
package.json
```

## commander

nodejs内置了对命令行操作的支持，node工程下package.json中的bin字段可以定义命令名和关联的执行文件。

``` json
{
  "name": "hxlz-create-app",
  "bin": {
    "hxlz": "./bin/hxlz.js"
  }
}
```

经过这样配置的nodejs项目，在使用-g选项进行全局安装的时候，会自动在系统的`[prefix]/bin`目录下创建相应的符号链接（symlink）关联到执行文件。如果是本地安装，这个符号链接会生成在`./node_modules/.bin`目录下。这样做的好处是可以直接在终端中像执行命令一样执行nodejs文件。关于`prefix`，可以通过`npm config get prefix`获取。

### 定义命令

在bin目录下创建一个hxlz.js文件，用于处理命令行的逻辑。

``` javascript
#!/usr/bin/env node

const program = require('commander')

program.version('1.0.0')
	.usage('<command> [项目名称]')
	.command('init', '创建新项目')
	.parse(process.argv)
```

### 定义子命令

ommander支持git风格的子命令处理，可以根据子命令自动引导到以特定格式命名的命令执行文件，文件名的格式是`[command]-[subcommand]`，例如：

hxlz init => hxlz-init

在bin目录下创建一个hxlz-init.js文件

``` javascript
#!/usr/bin/env node

const program = require('commander')

program.usage('<project-name>').parse(process.argv)

// 根据输入，获取项目名称
let projectName = program.args[0]

if (!projectName) {  // project-name 必填
  // 相当于执行命令的--help选项，显示help信息，这是commander内置的一个命令选项
  program.help()
  return
}

go()

function go () {
	// 预留，处理子命令  
}
```

## 使用download-git-repo下载模板

新建`lib/download.js`文件，用于下载模版

``` javascript
const download = require('download-git-repo')

/**
 * 下载模板
 * @param  {string} target 目标文件夹
 * @param  {string} repo   远程仓库
 */
module.exports = function (target, repo) {
  target = path.join(target || '.', '.download-temp')
  return new Promise((resolve, reject) => {
    download(repo,
        target, { clone: true }, (err) => {
      if (err) {
        reject(err)
      } else {
        // 下载的模板存放在一个临时路径中，下载完成后，可以向下通知这个临时路径，以便后续处理
        resolve(path.resolve(process.cwd(), target))
      }
    })
  })
}
```

使用Promise的风格处理异步

对`hxlz-init.js`进行修改

``` javascript
const download = require('./lib/download')

...
function go () {
  download(rootName)
    .then(target => console.log(target))
    .catch(err => console.log(err))
}
```

## 处理远程仓库中的meta.json文件，使用inquirer.js处理命令行交互

这里直接使用了vue-cli的`ask.js`文件

[文件地址](https://github.com/taohong000/hxlz-create-app/blob/master/lib/ask.js)

## 使用metalsmith处理模板
这里还用到了模版引擎`handlebars`
``` javascript
const Metalsmith = require('metalsmith')
const Handlebars = require('handlebars')

module.exports = function (metadata = {}, src, dest = '.') {
  if (!src) {
    return Promise.reject(new Error(`无效的source：${src}`))
  }

  return new Promise((resolve, reject) => {
    Metalsmith(process.cwd())
      .metadata(metadata)
      .clean(false)
      .source(src)
      .destination(dest)
      .use((files, metalsmith, done) => {
      	const meta = metalsmith.metadata()
        Object.keys(files).forEach(fileName => {
          const t = files[fileName].contents.toString()
          files[fileName].contents = new Buffer(Handlebars.compile(t)(meta))
        })
      	done()
      }).build(err => {
      	err ? reject(err) : resolve()
      })
  })
}
```

## 美化脚手架
通过一些工具包，让脚手架更加人性化。这里介绍两个在vue-cli中发现的工具包：

- ora - 显示spinner
- chalk - 给枯燥的终端界面添加一些色彩

> 参考链接
>
> [基于node.js平台的脚手架开发经历](http://zhangguoyu.org/2017/12/10/developing-a-cli-on-nodejs/)<br>
> [从零开始搭建前端脚手架](https://github.com/iq9891/blog/issues/2)
