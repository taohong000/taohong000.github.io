---
title: vuePress vue驱动的静态站点生成工具
categories:
  - 技术
date: 2018-04-25 13:56:59
tags:
	- 文档工具
	- vue
---

# vuePress vue驱动的静态站点生成工具

## 起步

### 全局安装

``` bash
# 全局安装
yarn global add vuepress # 或 npm install -g vuepress

# 创建一个 markdown 文件
echo '# Hello VuePress' > README.md

# 开始编写文档
vuepress dev

# 构建
vuepress build
```

### 在已有项目中安装

如果你想要在一个已有项目中维护文档，就应该将 VuePress 安装为本地依赖。此设置还允许你使用 CI 或 Netlify 服务，在推送时自动部署。

``` bash
# 安装为本地依赖项
yarn add -D vuepress # 或 npm install -D vuepress

# 创建一个 docs 目录
mkdir docs
# 创建一个 markdown 文件
echo '# Hello VuePress' > docs/README.md
```

然后，给 package.json 添加一些 scripts 脚本：

``` json
{
  "scripts": {
    "docs:dev": "vuepress dev docs",
    "docs:build": "vuepress build docs"
  }
}
```


## 主页(Homepage)

默认主题提供了一个主页布局（用于[该网站的主页](/)）。要使用它，需要在你的根目录 `README.md` 的 [YAML front matter](../guide/markdown.html#yaml-front-matter) 中指定 `home：true`，并加上一些其他的元数据。这是本网站使用的实际数据：

``` yaml
---
home: true
heroImage: /hero.png
actionText: 起步 →
actionLink: /guide/
features:
- title: 简明优先
  details: 对以 markdown 为中心的项目结构，做最简化的配置，帮助你专注于创作。
- title: Vue 驱动
  details: 享用 Vue + webpack 开发环境，在 markdown 中使用 Vue 组件，并通过 Vue 开发自定义主题。
- title: 性能高效
  details: VuePress 将每个页面生成为预渲染的静态 HTML，每个页面加载之后，然后作为单页面应用程序(SPA)运行。
footer: MIT Licensed | Copyright © 2018-present Evan You
---
```

`YAML front matter` 的内容之后的其他任意内容，将被解析为正常 markdown，并在 features 部分之后渲染。

如果你想彻底自定义主页的布局，你还可以使用[自定义布局](#custom-layout-for-specific-pages)

## 导航链接(navbar links)

你可以通过 `themeConfig.nav` 将链接添加到导航栏中：

``` js
// .vuepress/config.js
module.exports = {
  themeConfig: {
    nav: [
      { text: 'Home', link: '/' },
      { text: 'Guide', link: '/guide/' },
      { text: 'External', link: 'https://google.com' },
    ]
  }
}
```


## 文档链接

[官网](https://vuepress.docschina.org/)

[中文文档](https://vuepress.docschina.org/)

[参考项目](https://github.com/docschina/vuepress)
