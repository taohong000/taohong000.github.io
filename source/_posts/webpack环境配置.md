---
title: webpack环境配置
date: 2018-03-26 09:55:41
tags:
- webpack
- 构建工具
---

# webpack 环境配置
搭建开发环境主要有三种
1. webpack watch mode
2. webpack-dev-server
3. express + webpack-dev-middleware（更灵活，但是需要更多配置）

## webpack watch mode
清除打包内容

clean-webpack-plugin



## webpack dev server
1. live reloading（自动刷新浏览器）
2. 不能打包文件
3. 路径重定向
4. https
5. 浏览器中显示编译错误
6. 接口代理
7. 模块热更新

devServer
  inline
  contentBase
  port
  historyApiFallback
  https
  proxy
  hot
  openpage
  lazy
  overlay

historyApiFallback

对于单页面程序，浏览器的brower histroy可以设置成html5 history api或者hash，而设置为html5 api的，如果刷新浏览器会出现404 not found，原因是它通过这个路径（比如： /activities/2/ques/2）来访问后台，所以会出现404，而把historyApiFallback设置为true那么所有的路径都执行index.html



## 代理远程接口

proxy

代理远程接口请求

使用的是 http-proxy-middleware

options

| 配置选项      | 说明              |  
| :--------    | :--------         |
| target       | 代理地址           |
| changeOrigin | 改变你的源到这个url |
| headers      | http请求头         |
| logLevel     | 调试用的           |
| pathRewrite  | 重定向接口请求      |

## 模块热更新
Module Hot Reloading

- 保持应用的数据状态
- 节省调试时间
- 样式调试更快

如何设置
- devServer.hot
- webpack.HotModuleReplacementPlugin
- webpack.NamedModulesPlugin(查看模块的相对路径)

热门框架都有相应的热更新loader




## 开启调试 SourceMap

## 设置 ESLink 检查代码格式


## 区分开发环境 和 生产环境
- 开发环境
  - 模块热更新
  - sourceMap
  - 接口代理
  - 代码规范检查
- 生产环境
  - 提取公共代码
  - 压缩混淆
  - 文件压缩 或是 Base64 编码
  - 去除无用的代码

共同点
- 同样的入口哦
- 同样的代码处理（loader处理）
- 同样的解析配置

如何做
webpack-merge
webpack.dev.conf.js
webpack.prod.conf.js
webpack.common.conf.js

## 使用 middleware 来搭建开发环境

更灵活
