---
title: VS Code debug 配置
categories:
  - 技术
date: 2018-05-31 16:16:16
tags:
  - VS Code
  - debug
  - vue
---

下面是说明通过 [Vue CLI](https://github.com/vuejs/vue-cli) 生成的 Vue.js 应用程序中，如何使用 VS Code 的 [Debugger for Chrome](https://github.com/Microsoft/VSCode-chrome-debug) 扩展进行调试

## 先决条件

1. 安装好chrome 和 VSCode
2. 通过 vue-cli 创建项目

### 在 Chrome Devtools 中展示源代码

打开 `config/index.js` 并找到 `devtool` 属性。将其更新为：

``` 
devtool: 'source-map',
```

### 从 VS Code 启动应用

配置 `.vscode/launch.json`

``` json
{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "chrome",
      "request": "launch",
      "name": "vuejs: chrome",
      "url": "http://localhost:8080",
      "webRoot": "${workspaceFolder}/src",
      "breakOnLoad": true,
      "sourceMapPathOverrides": {
        "webpack:///src/*": "${webRoot}/*"
      }
    }
  ]
}
```

## 调试

1. 设置断点
2. 用终端工具输入 `npm start`启动项目 
3. 来到 Debug 视图，选择 ‘vuejs: chrome’ 配置，然后按 F5 或点击那个绿色的 play 按钮。
4. 随着一个新的 Chrome 实例打开 http://localhost:8080，你的断点现在应该被命中了。

我在调试中碰到了返回 500 Internal Privoxy Error 的情况，后来发现是代理工具 [shadowsocks](https://github.com/shadowsocks/shadowsocks-windows) 代理模式设置为**全局模式**的原因，把模式修改为 **PAC 模式**后便可正常调试了。

>参考链接
>
>[vue官方配置说明](https://cn.vuejs.org/v2/cookbook/debugging-in-vscode.html)

