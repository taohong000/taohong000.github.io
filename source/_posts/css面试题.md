---
title: css面试题
categories:
  - 技术
date: 2018-06-09 15:17:58
tags:
  - css
  - 面试
---

## 基础

### 如何确定css优先级

1. 选择器权重(id+100,类/属性/伪类+10,元素/伪元素+1,其他+0)
2. !important
3. 内联样式
4. 相同权重，后写的高

### 伪类和伪元素的区别
1. 伪类是状态，伪元素是元素
2. 伪类用`:`，伪元素用`::`

## 效果

### 如何用一个div画XXX

- box-shadow 无限投影
- ::before
- ::after

### 如何产生不占空间的边框

1. box-shadow
2. outline

## 动画

### transtion/keyframes 如何写

略

### css动画实现方式有几种

- transtion
- keyframes(animation)

### 过度动画和关键帧动画的区别

1. 过渡动画需要有状态变化
2. 关键帧动画不需要状态变化
3. 关键帧动画能控制更精细

### 如何实现逐帧动画

1. 使用关键帧动画
2. 去掉补间（steps）




