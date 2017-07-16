---
title: scss基础
date: 2017-07-16 19:11:57
tags: scss
---

# scss基础

@(SCSS)

找一个点看官网的demo入手，入手后边用边找对应的API。只要学会最基本的东西，如何编译，嵌套的写法，如何计算。就和学js一样，最开始只要学习变量，判断，循环，随着项目过程，不断的看api，慢慢就熟了，然后可以看一看背后的东西。
只要知道是什么，和最基本的东西就行。

前端技术发展的很快，要不断的学一些新东西，这样以这些东西为基础的新东西发展出来时，才能很快知道和学习。

[TOC]

## 变量
scss中可以定义变量，方便统一修改和维护。
``` scss
//scss style
//-----------------------------------
$fontStack:    Helvetica, sans-serif;
$primaryColor: #333;

body {
  font-family: $fontStack;
  color: $primaryColor;
}
```

``` css 
//css style
//-----------------------------------
body {
  font-family: Helvetica, sans-serif;
  color: #333;
}
```

## 嵌套
scss可以进行选择器的嵌套，表示层级关系，看起来很优雅整齐。

``` scss
//scss style
//-----------------------------------
nav {
  ul {
    margin: 0;
    padding: 0;
    list-style: none;
  }

  li { display: inline-block; }

  a {
    display: block;
    padding: 6px 12px;
    text-decoration: none;
  }
}
```

``` css 
//css style
//-----------------------------------
nav ul {
  margin: 0;
  padding: 0;
  list-style: none;
}

nav li {
  display: inline-block;
}

nav a {
  display: block;
  padding: 6px 12px;
  text-decoration: none;
}
```

## 导入

``` scss
scss中如导入其他scss文件，最后编译为一个css文件，优于纯css的@import
//scss style
//-----------------------------------
// _reset.scss

html,
body,
ul,
ol {
   margin: 0;
  padding: 0;
}
```

``` scss
//scss style
//-----------------------------------
// base.scss 

@import 'reset';

body {
  font-size: 100% Helvetica, sans-serif;
  background-color: #efefef;
}
```

``` css 
//css style
//-----------------------------------
html, body, ul, ol {
  margin: 0;
  padding: 0;
}

body {
  background-color: #efefef;
  font-size: 100% Helvetica, sans-serif;
}
```

## mixin
scss中可用mixin定义一些代码片段，且可传参数，方便日后根据需求调用。从此处理css3的前缀兼容轻松便捷。

``` scss
//scss style
//-----------------------------------
@mixin box-sizing ($sizing) {
    -webkit-box-sizing:$sizing;     
       -moz-box-sizing:$sizing;
            box-sizing:$sizing;
}
.box-border{
    border:1px solid #ccc;
    @include box-sizing(border-box);
}
```

``` css
//css style
//-----------------------------------
.box-border {
  border: 1px solid #ccc;
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
}
```

## 扩展/继承
scss可通过@extend来实现代码组合声明，使代码更加优越简洁。

``` scss 
//scss style
//-----------------------------------
.message {
  border: 1px solid #ccc;
  padding: 10px;
  color: #333;
}

.success {
  @extend .message;
  border-color: green;
}

.error {
  @extend .message;
  border-color: red;
}

.warning {
  @extend .message;
  border-color: yellow;
}
```

``` css 
//css style
//-----------------------------------
.message, .success, .error, .warning {
  border: 1px solid #cccccc;
  padding: 10px;
  color: #333;
}

.success {
  border-color: green;
}

.error {
  border-color: red;
}

.warning {
  border-color: yellow;
}
```

## 运算
scss可进行简单的加减乘除运算等

``` scss 
//scss style
//-----------------------------------
.container { width: 100%; }

article[role="main"] {
  float: left;
  width: 600px / 960px * 100%;
}

aside[role="complimentary"] {
  float: right;
  width: 300px / 960px * 100%;
}
```

``` css 
//css style
//-----------------------------------
.container {
  width: 100%;
}

article[role="main"] {
  float: left;
  width: 62.5%;
}

aside[role="complimentary"] {
  float: right;
  width: 31.25%;
}
```

## 颜色
scss中集成了大量的颜色函数，让变换颜色更加简单。

``` scss 
//scss style
//-----------------------------------
$linkColor: #08c;
a {
    text-decoration:none;
    color:$linkColor;
    &:hover{
      color:darken($linkColor,10%);
    }
}
```

``` css 
//css style
//-----------------------------------
a {
  text-decoration: none;
  color: #0088cc;
}
a:hover {
  color: #006699;
}
```


## compass
compass由scss的核心团队成员Chris Eppstein创建，是一个非常丰富的样式框架，包括大量定义好的mixin，函数，以及对scss的扩展。

