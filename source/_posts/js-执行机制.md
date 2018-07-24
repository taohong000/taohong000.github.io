---
title: js 执行机制
categories:
  - 技术
date: 2018-07-24 18:17:19
tags: 
  - js
---

## 概览
![normal](js-执行机制/js执行机制概览.png)

## 任务队列

Js 中，有两类任务队列：宏任务队列（macro tasks）和微任务队列（micro tasks）。宏任务队列可以有多个，微任务队列只有一个。那么什么任务，会分到哪个队列呢？

- 宏任务：script（全局任务）, setTimeout, setInterval, setImmediate, I/O, UI rendering.
- 微任务：process.nextTick, Promise, Object.observer, MutationObserver.

## 执行顺序
先执行主线程，再取微任务，再取宏任务
