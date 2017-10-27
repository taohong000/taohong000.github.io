---
title: DOM事件
date: 2017-08-24 16:35:22
tags:
- css
- 基础
---

# DOM事件

## DOM事件的级别
DOM0, element.onclick = function(){}
DOM2, element.addEventListener(’click’, ()=>{}, false)
DOM3, element.addEventListener(‘keyup’, ()=>{}, false)

## DOM事件流
捕获阶段，目标阶段，冒泡阶段。

## 描述DOM事件捕获的具体流程
window --> document —>html —> body —> …… —> target

如何获取html标签:
document.body ==> 获取body
document.documentElement ==> 获取html

## Event对象的常见应用
event.preventDefault()
event.stopPropagation()
event.stopImmediatePropagation()
event.currentTarget
event.target

 target，currentTarget和this三者的区别
 >target在事件流的目标阶段；
 >currentTarget在事件流的捕获，目标及冒泡阶段。
 >只有当事件流处在目标阶段的时候，两个的指向才是一样的， 而当处于捕获和冒泡阶段的时候，target指向被单击的对象而currentTarget指向当前事件活动的对象(注册该事件的对象)（一般为父级）。this指向永远和currentTarget指向一致（只考虑this的普通函数调用）。

## 自定义事件
``` javascript
// 创建自定义事件
var eve = new Event('custome');
ev.addEventListener('custome', ()=>{})
// 触发自定义事件
ev.dispatch(eve)
```

JS(原生)事件委托：为动态创建的节点绑定事件
[JS(原生)事件委托：为动态创建的节点绑定事件](http://www.cnblogs.com/chengyanfen/p/3716163.html)