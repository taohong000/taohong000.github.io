---
title: sublime常用插件
date: 2017-07-16 18:52:02
tags: sublime
---

# sublime常用插件

@(sublime)[插件, 初始化配置]

[TOC]

## 设置(setting)

```json
{
	"auto_complete_triggers":
	[
		{
			"characters": "1234567890abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ.<",
			"selector": "text.html"
		}
	],
	"font_size": 14,
	"ignored_packages":
	[
		"Vintage"
	],
	"tab_size": 2
}
```


## 插件

### 安装插件
```
ctrl+shirt+p
```
install package control

### 常用插件

#### emmet
介绍：仿选择器语法生成代码

#### docBlock
介绍：自动补全注释

#### html/css/js prettify
介绍：整理代码

#### CSScomb
介绍：规范css，整理css顺序
配置网站：http://csscomb.com/config
配置：

```json
{
    "node-path" : "C:\\Program Files\\nodejs.node.exe",
    "config": {
        "remove-empty-rulesets": true,
        "always-semicolon": true,
        "color-case": "lower",
        "block-indent": "\t",
        "color-shorthand": true,
        "element-case": "lower",
        "eof-newline": false,
        "leading-zero": false,
        "quotes": "single",
        "sort-order-fallback": "abc",
        "space-before-colon": "",
        "space-after-colon": " ",
        "space-before-combinator": " ",
        "space-after-combinator": " ",
        "space-between-declarations": "\n",
        "space-before-opening-brace": " ",
        "space-after-opening-brace": "\n",
        "space-after-selector-delimiter": "\n",
        "space-before-selector-delimiter": "",
        "space-before-closing-brace": "\n",
        "strip-spaces": true,
        "tab-size": true,
        "unitless-zero": true,
        "vendor-prefix-align": true
    }
}
```

#### file header
介绍：添加文件header

#### jQuery
介绍：jquery代码提示

#### Bootstrap 3 Snippets
介绍：bootstrap3 代码片段

#### SCSS
介绍：scss代码提示

#### ES6-Toolkit 
介绍：es6转es5,es6代码片段

#### SideBarEnhancements
介绍：打开浏览器

#### Terminal
介绍：打开命令行工具