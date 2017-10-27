---
title: 常用npm包
date: 2017-10-27 16:58:31
categories: 
- 技术
tags:
- npm
---

# 常用npm包

## cross-env
介绍：解决跨平台设置NODE_ENV的问题。
这个迷你的包能够提供一个设置环境变量的scripts，让你能够以lunix方式设置环境变量，然后在windows上也能兼容运行。


使用方法：
- 安装cross-env:npm install cross-env --save-dev
- 在NODE_ENV=xxxxxxx前面添加cross-env就可以了。

``` javascript
{
  "scripts": {
    "build": "cross-env NODE_ENV=production webpack --config build/webpack.config.js"
  }
}
```

