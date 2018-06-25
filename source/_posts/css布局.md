---
title: css布局
categories:
  - 技术
date: 2018-06-09 15:36:57
tags: 
  - css
---

## 布局重点

1. table布局
2. 技巧性布局
3. flexbox/grid布局
4. 响应式布局

## 盒模型
content + padding + border + margin

? box-sizing

## display/position

display 确定元素的显示类型

block/inline/inline-block

position 确定元素的位置

static/relative/absolute/fixed

## flexbox 布局

## float + margin 布局

- 元素“浮动”
- 脱离文档流
- 但不脱离文本流

### 对自身对影响
- 形成“块”（BFC）
- 位置尽量考上
- 位置尽量靠左（右）

### 对兄弟元素的影响

- 上面贴非float元素（一般）
- 旁边贴float元素
- 不影响其他块级元素的位置
- 影响其他块级元素的内部文本

### 对父级

- 从布局上消失
- 高度塌陷

### 修复高度塌陷

1. 把父元素变为BFC（如加上 overflow：auto）
2. 添加元素（一般加上伪元素）

### 三栏布局

用 float + margin
``` html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
        .container{
            width:800px;
            height:200px;
        }
        .left{
            background:red;
            /* float:left; */
            /* height:100%; */
            width:200px;
            position: absolute;
            height:200px;
        }
        .right{
            background:blue;
            float:right;
            width:200px;
            height:100%;
        }
        .middle{
            margin-left:200px;
            margin-right:200px;
        }
        
    </style>
</head>
<body>
    <div class="container">
        <div class="left">
            左
        </div>
        <div class="right">
            右
        </div>
        <div class="middle">
            中间
        </div>
    </div>
</body>
</html>
```

布局的核心是--如何进行元素的横向排列

## inline-block 布局

1. 像文本一样排 block 元素
2. 没有清除浮动的问题
3. 需要处理间隙
  1. 父元素字体大小设为0
  2. 去除元素间的空白

## 响应式设计和布局

1. 在不同设备上正常使用
2. 一般处理大小问题
3. 主要方法
  1. 隐藏 + 折行 + 自适应空间
  2. rem/viewprot/media query



