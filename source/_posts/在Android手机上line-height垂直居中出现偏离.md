---
title: 在Android手机上line-height垂直居中出现偏离
categories:
  - 技术
date: 2018-07-23 10:38:35
tags:
---

## 原因

> 导致这个问题的本质原因可能是Android在排版计算的时候参考了primyfont字体的相关属性（即HHead Ascent、HHead Descent等），而primyfont的查找是看`font-family`里哪个字体在fonts.xml里第一个匹配上，而原生Android下中文字体是没有family name的，导致匹配上的始终不是中文字体，所以解决这个问题就要在`font-family`里显式申明中文，或者通过什么方法保证所有字符都fallback到中文字体。根据这2个思路，目前我找到了2个解决方案：
> 1. 针对Android 7.0+设备：`<html>`上设置 lang 属性：`<html lang="zh-cmn-Hans">`，同时font-family不指定英文，如 font-family: sans-serif 。这个方法是利用了浏览器的字体fallback机制，让英文也使用中文字体来展示，blink早期的内核在fallback机制上存在问题，Android 7.0+才能ok，早期的内核下会导致英文fallback到Noto Sans Myanmar，这个字体非常丑。
> 2. 针对MIUI 8.0+设备：设置 font-family: miui 。这个方案就是显式申明中文的方案，MIUI在8.0+上内置了小米兰亭，同时在fonts.xml里给这个字体指定了family name：miui，所以我们可以直接设置。
> 
> 作者：周祺
链接：https://www.zhihu.com/question/39516424/answer/274374076

![normal](在Android手机上line-height垂直居中出现偏离/normal.png)

## 文字明显偏上的情况
在 Android 设备上文字明显偏上的情况一般有两种:
1. 字体小于12px
2. 文字, 行高, 容器高度为奇数

![question](在Android手机上line-height垂直居中出现偏离/question.png)

## 各个居中方法的对比

### 字体小于12px

![answer1](在Android手机上line-height垂直居中出现偏离/answer1.png)

结论: 放大缩小法和添加border有效.

### 文字, 行高, 容器高度为奇数

![answer2](在Android手机上line-height垂直居中出现偏离/answer2.png)

结论: 修改容器高度, 放大缩小法和添加border有效.






