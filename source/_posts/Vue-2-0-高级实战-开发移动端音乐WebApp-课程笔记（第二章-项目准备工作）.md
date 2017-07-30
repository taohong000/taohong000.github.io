---
title: Vue 2.0 高级实战-开发移动端音乐WebApp 课程笔记（第二章 项目准备工作）
date: 2017-07-18 23:44:27
categories: 
- 技术
- 课程笔记
tags: 
- 慕课网
- vue
---

# Vue 2.0 高级实战-开发移动端音乐WebApp 课程笔记（第二章 项目准备工作）

## 需求分析

## vue-cli脚手架安装
``` bash
vue init webpack vue-music
```

## 项目目录介绍及图标字体、公共样式等资源准备
src目录介绍
- src
	- api （后端请求相关代买）
	- common （通用静态资源）
		- fonts
		- image
		- js
		- stylus
			- base.styl （基础样式）
			- icon.styl （图标字体文件）
			- index.styl （样式入口文件）
			- mixin.styl （函数）
			- reset.styl （重置样式文件）
			- variable.styl （变量配置）
	- components （通用组件）
	- router （路由相关文件）
	- store （vuex相关代码）
	- App.vue
	- main.js （入口文件）

variable.styl：设计都有一定的规范，保证风格统一。这个文件定义了颜色规范和字体规范。可以方便知道开发用什么样的颜色，保证开发的方便性。
