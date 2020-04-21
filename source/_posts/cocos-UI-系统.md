---
title: cocos UI 系统
categories:
  - 技术
date: 2020-04-03 12:06:42
tags:
  - 游戏
---

> 参考文档：[cocos 文档](https://docs.cocos.com/creator/manual/zh/)

## 基本元素
1. Sprite(精灵) 就是图片
2. Label(文字) 

## 基本布局
### Widget(对齐挂件)
操作元素相对目标元素(默认父元素)的定位, 相当于css的绝对定位

### Layout(自动布局)
用于元素排列布局
type: 控制元素方向 横排, 竖排, 网格
padding: 和父元素的边距
Spacing: 元素间的间距
direction: 排列方向

### scrollView(滚动视图)
目录结构
scrollView
 - scrollBar 滚动条
 - view 上面添加了 mask 组件, 这样只显示元素内的内容
 	- content 滚动元素容器
 	- item 滚动元素

### Prefab(预制资源)
把视图变成资源, 相当于组件
