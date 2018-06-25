---
title: VS code ESLint 配置
categories:
  - 技术
date: 2018-05-31 17:20:27
tags:
  - VS Code
  - ESLint
  - vue
---

下面是说明如何在 VS Code 中对 vue 代码进行检查和自动修复

## 安装插件

安装 VS code 插件 [vetur](https://github.com/vuejs/vetur) 和 [ESLint](https://github.com/Microsoft/vscode-eslint)

- vetur 是 vue 官方的插件，集成了许多开发必要的功能
- ESlint 是 检查工具

## 配置 浏览器设置

``` json
{
  "eslint.validate": [
      "javascript",
      "javascriptreact",
      // 支持 *.vue
      {
          "language": "vue",
          "autoFix": true
      },
  ],
  // 保存时设置文件的格式。格式化程序必须可用，不能自动保存文件，并且不能关闭编辑器。
  "editor.formatOnSave": true,
   // 在保存和关闭时自动修复代码
  "eslint.autoFixOnSave": true,
  // Vetur 自动使用 eslint-plugin-vue 检测 <template>。Linting配置基于 eslint-plugin-vue 的基本规则集。这是成 false 关闭它
  "vetur.validation.template": false
}
```

## 安装依赖

```
yarn add -D eslint eslint-plugin-vue
```

## 设置 ESLint 规则。下面是一个例子：

在 `.eslintrc` 中配置

``` json
{
  "extends": [
    "eslint:recommended",
    "plugin:vue/recommended"
  ],
  "rules": {
    "vue/html-self-closing": "off"
  }
}

```

>参考链接
>
>[Vetur Linting 配置](https://vuejs.github.io/vetur/linting-error.html)