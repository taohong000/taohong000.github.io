---
title: webpack常见面试题
categories:
  - 技术
date: 2018-06-08 11:07:17
description: 介绍常见webpack面试题，分为 1. 概念 2. 配置 3. 开发 4. 优化 四方面。
tags:
  -webpack
---

## 面试类型

- 概念
- 配置
- 开发
- 优化

## 概念

### 什么是 webpack，它和 grunt/glup 有什么不同？

webpack 是一个模块打包器，它可以递归的打包项目中的所有模块，最终生成几个打包后的文件。它和其它工具最大的不同在于它支持 code-splitting、模块化（AMD，ESM，CommonJS）、全局分析。

grunt/glup 是自动化构建工具，是做任务管理的。两者区别：glup是基于流做任务管理的。相对来说 glup api 更少，使用更简单。

### 什么是 bundle，什么是 chunk，什么是 module？

bundle 是webpack 打包出来的文件，chunk 是指 webpack 在进行模块的依赖分析的时候，代码分割出来的代码块，module 是开发过程中的单个模块。

### 什么是 Loader？什么是 Plugin？

Loader 是用来告诉 webpack 如何转化处理某一类的文件，并且引入到打包文件中。

Plugin 是用来自定义 webpack 打包过程的方式，一个插件是含有apply方法的一个对象，通过这个方法可以参与到整个 webpack 打包的各个流程（生命周期）。

## 配置

### 如何可以自动生成 webpack 配置？

webpack-cli/vue-cli/etc... 脚手架工具

## 开发

### webpack-dev-server 和 http 服务器如 nginx 有什么区别？

webpack-dev-server 使用内存来存储 webpack 开发环境下的打包文件，并且可以使用模块热更新，他比传统的 http 服务对开发更加简单高效。

webpack-dev-server 就是 express + webpack-hot-middleware + 一些配置

### 什么是模块热更新？

模块热更新是 webpack 的一个功能，它可以使得代码修改过后不用刷新浏览器就可以更新，是高级版的自动刷新浏览器。

## 优化

### 什么是长缓存？在 webpack 中如何做到长缓存优化？

浏览器在用户访问页面的时候，为了加快加载速度，会对用户访问的静态资源进行存储，但每一次代码升级或是更新，都需要浏览器去下载新的代码，最方便和简单的更新方式就是引入新的文件名称。

在 webpack 中可以在 output 给输出的文件指定 chunkhash，并且分离经常更新的代码和框架代码。通过 NamedModulesPlugin 或是 HashedModuleIdsPlugin 使再次打包文件名不变。

### 什么是 Tree-shaking？CSS 可以 Tree-shaking 吗？
Tree-shaking 是指在打包中去除那些引入了，但是在代码中没有被用到的那些死代码。在 webpack 中 Tree-shaking 是通过 uglifyJSPlugin 来 Tree-shaking JS。CSS 需要使用 Purify-CSS。

>参考链接
>
>[四大维度解锁 Webpack 3.0 前端工程化](https://coding.imooc.com/learn/list/171.html)













