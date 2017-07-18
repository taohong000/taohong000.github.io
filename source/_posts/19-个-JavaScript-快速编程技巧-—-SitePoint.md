---
title: 19+ 个 JavaScript 快速编程技巧 — SitePoint
date: 2017-07-18 10:27:33
categories: 
- 技术
tags:
- js
---

# 19+ 个 JavaScript 快速编程技巧 — SitePoint

> 本文转载自：[众成翻译](http://www.zcfy.cc)
> 译者：[myvin](http://www.zcfy.cc/@myvin)
> 链接：[http://www.zcfy.cc/article/3519](http://www.zcfy.cc/article/3519)
> 原文：[https://www.sitepoint.com/shorthand-javascript-techniques/](https://www.sitepoint.com/shorthand-javascript-techniques/)

![improve-your-javascript](http://p0.qhimg.com/t015c23b70ee9d2ef75.jpg)

**这确实是一篇针对于基于 JavaScript 语言编程的开发者必读的文章**。在过去几年我学习 JavaScript 的时候，我写下了这篇文章，并将其作为 JavaScript 快速编程技巧的一个重要参考。为了有助于理解，针对常规写法我也给出了相关的编程观点。

> **2017 年 6 月 14 日**：这篇文章更新了一些基于 ES6 的速记写法。如果你想进一步了解 ES6 中有哪些新增的变化，可以注册 SitePoint Premium 并查看我们录制的视频[A Look into ES6](https://www.sitepoint.com/premium/screencasts/a-look-into-es2016)。

## 1. 三元操作符

如果你想只用一行代码写出一个 if..else 表达式，那么这是一个很好的节省代码的方式。

常规写法：

``` javascript
const x = 20;
let answer;
if (x > 10) {
    answer = 'is greater';
} else {
    answer = 'is lesser';
}

```

速记法：

``` javascript
const answer = x > 10 ? 'is greater' : 'is lesser';

```

你也可以像这样嵌套 if 表达式：

``` javascript
const big = x > 10 ? " greater 10" : x

```

## 2. 短路求值速记法

当需要给另一个变量分配一个变量时，你可能需要确保变量不是 null、undefined 或者不为空。你可以写一个有多个 if 表达式的语句，你也可以使用短路求值。

常规写法：

``` javascript
if (variable1 !== null || variable1 !== undefined || variable1 !== '') {
     let variable2 = variable1;
}

```

速记法：

``` javascript
const variable2 = variable1  || 'new';

```

你不相信这样可以 work？那就自己测试下吧（把下面的代码复制粘贴到 [es6console](https://es6console.com/)）：

``` javascript
let variable1;
let variable2 = variable1  || '';
console.log(variable2 === ''); // prints true

variable1 = 'foo';
variable2 = variable1  || '';
console.log(variable2); // prints foo

```

## 3. 变量声明速记法

在函数里声明变量时，如果需要同时声明多个变量，这种速记法能够给你节省大量的时间和空间。

常规写法：

``` javascript
let x;
let y;
let z = 3;

```

速记法：

``` javascript
let x, y, z=3;

```

## 4. If 判断变量是否存在速记法

这可能会有些琐碎，但是值得一提。当需要用 if 判断一个变量是否为真时，赋值运算符有时候可以省略。

常规写法：

``` javascript
if (likeJavaScript === true)

```

速记法：

``` javascript
if (likeJavaScript)

```

> **注意：**这两个例子并不是完全相等，只要 `likeJavaScript` 变量是一个 [真值](https://developer.mozilla.org/en-US/docs/Glossary/Truthy)，该表达式就是成立的。

再给出一个例子。如果 "a" 不等于 true，如下：

常规写法：

``` javascript
let a;
if ( a !== true ) {
// do something...
}

```

速记法：

``` javascript
let a;
if ( !a ) {
// do something...
}

```

## 5. JavaScript 循环速记法

如果你只想跑原生 JavaScript，不想依赖如 JQuery 或 lodash 这样的外部库，那这个小技巧会非常有用。

常规写法：

``` javascript
for (let i = 0; i < allImgs.length; i++)

```

速记法：

``` javascript
for (let index in allImgs)

```

Array.forEach 速记法：

``` javascript
function logArrayElements(element, index, array) {
  console.log("a[" + index + "] = " + element);
}
[2, 5, 9].forEach(logArrayElements);
// logs:
// a[0] = 2
// a[1] = 5
// a[2] = 9

```

## 6. 短路求值

如果我们不想为了只是判断一个变量是 null 或 undefined 就分配一个默认值而写六行代码，那么可以使用短路逻辑操作符完成同样的功能，而且只有一行代码。

常规写法：

``` javascript
let dbHost;
if (process.env.DB_HOST) {
  dbHost = process.env.DB_HOST;
} else {
  dbHost = 'localhost';
}

```

速记法：

``` javascript
const dbHost = process.env.DB_HOST || 'localhost';

```

## 7. 十进制基数指数

你可能随处可见这种写法。这是一种比较 fancy 的写法，省去了后面的一堆零。举个栗子，1e7 就意味着 1 后面跟着 7 个零。这是十进制基数指数的一种写法（JavaScript 会按照浮点类型去解释），和 10,000,000 是相等的。

常规写法：

``` javascript
for (let i = 0; i < 10000; i++) {}

```

速记法：

``` javascript
for (let i = 0; i < 1e7; i++) {}

// All the below will evaluate to true
1e0 === 1;
1e1 === 10;
1e2 === 100;
1e3 === 1000;
1e4 === 10000;
1e5 === 100000;

```

## 8. 对象属性速记法

在 JavaScript 中定义对象字面量非常简单。ES6 提供了一个更简单的定义对象属性的方法。如果 name 和 key 名字相同，那么就可以直接使用如下速记法。

常规写法：

``` javascript
const obj = { x:x, y:y };

```

速记法：

``` javascript
const obj = { x, y };

```

## 9. 箭头函数速记法

经典的函数写法易于阅读，但是一旦将这样的函数放进回调中就会略显冗长，而且会造成一些困惑。

常规写法：

``` javascript
function sayHello(name) {
  console.log('Hello', name);
}

setTimeout(function() {
  console.log('Loaded')
}, 2000);

list.forEach(function(item) {
  console.log(item);
});

```

速记法：

``` javascript
sayHello = name => console.log('Hello', name);

setTimeout(() => console.log('Loaded'), 2000);

list.forEach(item => console.log(item));

```

这里需要注意的是：`this` 值在箭头函数和常规写法的函数里是完全不同的，所以那两个例子并不是严格等价的。查看 [this article on arrow function syntax](https://www.sitepoint.com/es6-arrow-functions-new-fat-concise-syntax-javascript/)获取更多细节。

## 10. 隐性返回速记法

我们经常使用 return 关键字来返回一个函数的结果。仅有一个表达式的箭头函数会隐性返回函数结果（函数必须省略大括号(`{}`)才能省略 return 关键字）。

如果要返回多行表达式（比如一个对象字面量），那么需要用 `()` 而不是 `{}` 来包裹函数体。这样可以确保代码作为一个单独的表达式被计算返回。

常规写法：

``` javascript
function calcCircumference(diameter) {
  return Math.PI * diameter
}

```

速记法：

``` javascript
calcCircumference = diameter => (
  Math.PI * diameter;
)

```

## 11. 默认参数值

你可以使用 if 表达式为函数参数定义默认值。在 ES6 中，你可以在函数声明的时候直接定义默认值。

常规写法：

``` javascript
function volume(l, w, h) {
  if (w === undefined)
    w = 3;
  if (h === undefined)
    h = 4;
  return l * w * h;
}

```

速记法：

``` javascript
volume = (l, w = 3, h = 4 ) => (l * w * h);

volume(2) //output: 24

```

## 12. 模板字面量

你是不是已经厌倦了使用 `' + '` 来将多个变量拼接成一个字符串？难道就没有更简单的方式来完成吗？如果你可以使用 ES6 的话，那么恭喜你，你要做的只是使用反引号和 `${}` 来包裹你的变量。

常规写法：

``` javascript
const welcome = 'You have logged in as ' + first + ' ' + last + '.'

const db = 'http://' + host + ':' + port + '/' + database;

```

速记法：

``` javascript
const welcome = `You have logged in as ${first} ${last}`;

const db = `http://${host}:${port}/${database}`;

```

## 13. 解构赋值速记法

如果你正在使用任意一种流行的 web 框架，那么你很有可能会使用数组或者对象字面量形式的数据在组件和 API 之间传递信息。一旦组件接收到数据对象，你就需要将其展开。

常规写法：

``` javascript
const observable = require('mobx/observable');
const action = require('mobx/action');
const runInAction = require('mobx/runInAction');

const store = this.props.store;
const form = this.props.form;
const loading = this.props.loading;
const errors = this.props.errors;
const entity = this.props.entity;

```

速记法：

``` javascript
import { observable, action, runInAction } from 'mobx';

const { store, form, loading, errors, entity } = this.props;

```

你甚至可以给变量重新分配变量名：

``` javascript
const { store, form, loading, errors, entity:contact } = this.props;

```

## 14. 多行字符串速记法

如果你需要在代码中写多行字符串，那么你可能会这样写：

常规写法：

``` javascript
const lorem = 'Lorem ipsum dolor sit amet, consectetur\n\t'
    + 'adipisicing elit, sed do eiusmod tempor incididunt\n\t'
    + 'ut labore et dolore magna aliqua. Ut enim ad minim\n\t'
    + 'veniam, quis nostrud exercitation ullamco laboris\n\t'
    + 'nisi ut aliquip ex ea commodo consequat. Duis aute\n\t'
    + 'irure dolor in reprehenderit in voluptate velit esse.\n\t'

```

但是有一种更简单的方法：使用反引号。

速记法：

``` javascript
const lorem = `Lorem ipsum dolor sit amet, consectetur
    adipisicing elit, sed do eiusmod tempor incididunt
    ut labore et dolore magna aliqua. Ut enim ad minim
    veniam, quis nostrud exercitation ullamco laboris
    nisi ut aliquip ex ea commodo consequat. Duis aute
    irure dolor in reprehenderit in voluptate velit esse.`

```

## 15. 展开运算符速记

**展开运算符**是在 ES6 中引入的，它的多种应用场景使得 JavaScript 代码使用起来更高效、更有趣。它可以用来替换某些数组函数。展开运算符写起来很简单，就是三个点。

常规写法：

``` javascript
// joining arrays
const odd = [1, 3, 5];
const nums = [2 ,4 , 6].concat(odd);

// cloning arrays
const arr = [1, 2, 3, 4];
const arr2 = arr.slice()

```

速记法：

``` javascript
// joining arrays
const odd = [1, 3, 5 ];
const nums = [2 ,4 , 6, ...odd];
console.log(nums); // [ 2, 4, 6, 1, 3, 5 ]

// cloning arrays
const arr = [1, 2, 3, 4];
const arr2 = [...arr];

```

和 `concat()` 函数不同，你可以在另一个数组里的任意位置插入一个数组。

``` javascript
const odd = [1, 3, 5 ];
const nums = [2, ...odd, 4 , 6];

```

你也可以将展开运算符和 ES6 解析赋值结合起来使用：

``` javascript
const { a, b, ...z } = { a: 1, b: 2, c: 3, d: 4 };
console.log(a) // 1
console.log(b) // 2
console.log(z) // { c: 3, d: 4 }

```

## 16. 强制参数速记法

如果没有传值的话，JavaScript 默认会将函数参数设置为 `undefined`。一些其他的编程语言会抛出警告或错误。为了强制给参数赋值，如果参数没有定义的话，你可以使用 `if` 表达式抛出错误，或者可以使用“强制参数速记法”。

常规写法：

``` javascript
function foo(bar) {
  if(bar === undefined) {
    throw new Error('Missing parameter!');
  }
  return bar;
}

```

速记法：

``` javascript
mandatory = () => {
  throw new Error('Missing parameter!');
}

foo = (bar = mandatory()) => {
  return bar;
}

```

## 17. Array.find 速记法

如果你曾经使用原生 JavaScript 写一个查找函数，你可能会使用 for 循环。在 ES6 中，你可以使用数组的一个新方法 `find()`。

常规写法：

``` javascript
const pets = [
  { type: 'Dog', name: 'Max'},
  { type: 'Cat', name: 'Karl'},
  { type: 'Dog', name: 'Tommy'},
]

function findDog(name) {
  for(let i = 0; i<pets.length; ++i) {
    if(pets[i].type === 'Dog' && pets[i].name === name) {
      return pets[i];
    }
  }
}

```

速记法：

``` javascript
pet = pets.find(pet => pet.type ==='Dog' && pet.name === 'Tommy');
console.log(pet); // { type: 'Dog', name: 'Tommy' }

```

## 18. Object [key] 速记法

你知道 `Foo.bar` 可以写成 `Foo['bar']` 吧。一开始，似乎并没有原因解释说为什么应该像这样写。但是这种写法可以让你编写可重用代码。

考虑下一个验证函数的简单例子：

``` javascript
function validate(values) {
  if(!values.first)
    return false;
  if(!values.last)
    return false;
  return true;
}

console.log(validate({first:'Bruce',last:'Wayne'})); // true

```

这个函数完美的实现了所需的功能。但是，请考虑一个场景：你有许多表单需要验证，并且不同的域有不同的验证规则。那创建一个在运行时被配置的通用验证函数岂不是更好？

速记法：

``` javascript
// object validation rules
const schema = {
  first: {
    required:true
  },
  last: {
    required:true
  }
}

// universal validation function
const validate = (schema, values) => {
  for(field in schema) {
    if(schema[field].required) {
      if(!values[field]) {
        return false;
      }
    }
  }
  return true;
}

console.log(validate(schema, {first:'Bruce'})); // false
console.log(validate(schema, {first:'Bruce',last:'Wayne'})); // true

```

现在创建了一个可以在所有的表单里重用的验证函数，而不必为每个表单单独写一个特定的验证函数。

## 19. 双位取反运算符速记法

逐位运算符是你在刚学习 JavaScript 时会学到的一个特性，但是如果你不处理二进制的话，基本上是从来都不会用上的。

但是，双位运算符有一个非常实用的使用场景：可以用来代替 `Math.floor`。双位取反运算符的优势在于它执行相同操作的速度更快。你可以在[这里](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Bitwise_Operators)查看更多关于位运算符的知识。

常规写法：

``` javascript
Math.floor(4.9) === 4  //true

```

速记法：

``` javascript
~~4.9 === 4  //true

```

## 20. 还有其他的小技巧？

我确实喜欢这些小技巧，也乐于发现更多的小技巧。如果你有什么想说的话，就直接留言吧！