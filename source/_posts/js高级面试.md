---
title: js高级面试
categories:
  - 技术
tags:
  - js
  - 面试
date: 2018-06-11 09:25:42
---


## es6

### 模块化的使用和编译环境

...

### Class 与 JS 构造函数的区别

### Promise的用法

1. new Promise 实例，而且要 return
2. new Promise 时要传入函数，函数有 reslove reject 两个参数
3. 成功时执行 resolve() 失败时执行 reject()
4. then 监听结果

### ES6 其他常用功能

1. let/const
2. 多行字符串/模版变量
3. 结构赋值
4. 块级作用域
5. 函数默认参数
6. 箭头函数



## 异步

### 什么是单线程，和异步有何关系

- 单线程 - 只有一个线程，同时只能做一件事
- 原因 - 避免 DOM 渲染的冲突
- 解决方案 - 异步

### 什么是 event-loop

- 事件轮询，JS 实现异步的具体解决方案
- 同步代码，直接执行
- 异步函数先放在 异步队列 中
- 待同步函数执行完毕，轮询执行 异步队列 的函数

JS中的异步操作：
1. 定时器都是异步操作
2. 事件绑定都是异步操作
3. AJAX中一般我们都采取异步操作（也可以同步）
4. 回调函数可以理解为异步（不是严谨的异步操作）
剩下的都是同步处理

### Promise 的标准

all/race
- Promise.all 待全部完成后,执行then
- Promise.race 一个promise 完成后,执行then


状态

三种状态：pending fulfilled rejected

then
- Promise 实例必须实现 then 这个方法
- then() 必须可以接收两个函数作为参数
- then() 返回的必须是一个 Promise 实例


### async/await 的使用

- 基本语法
- 使用了 Promise ，并没有和 Promise 冲突
- 完全是同步的写法，再也没有回调函数
- 但是：改变不了 JS 单线程、异步的本质

## 原型

### 原型如何实际应用

### 原型如何满足扩展

## vdom

...

## MVVM

### 之前使用jquery和现在使用vue或React框架的区别
- 数据和视图的分离，解耦（开放封闭原则）
- 以数据驱动视图，只关心数据变化，DOM 操作被封装

### 你如何理解MVVM

- Model - 模型、数据
- View - 视图、模板（视图和模型是分离的）
- ViewModel - 连接 Model 和 View 

### vue 如何实现响应式

vue 三要素
- 响应式：vue 如何监听到 data 的每个属性变化？
- 模板引擎：vue 的模板如何被解析，指令如何处理？
- 渲染：vue 的模板如何被渲染成 html ？以及渲染过程

答案
- 关键是理解 Object.defineProperty
- 将 data 的属性代理到 vm 上

### vue 如何解析模版

- 模板：字符串，有逻辑，嵌入 JS 变量……
- 模板必须转换为 JS 代码（有逻辑、渲染 html、JS 变量）
- render 函数是什么样子的
- render 函数执行是返回 vnode
- updateComponent

### 介绍 vue 的实现流程

1. 解析模板成 render 函数
2. 响应式开始监听
3. 首次渲染，显示页面，且绑定依赖
4. data 属性变化，触发 rerender

## 组件化

...
