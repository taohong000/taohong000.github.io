---
title: 格式化上下文(Formatting Context)
categories:
  - 技术
date: 2018-07-25 16:29:36
tags:
  - css
  - 基础
---

## 基础概念
### Box

一个页面是由很多个 `Box` 组成的，元素的类型和 `display` 属性决定了这个 `Box` 的类型。不同类型的 `Box`，会参与不同的 `Formatting Context`。

`Block level` 的 `box` 会参与形成 `BFC`，比如 `display` 值为 `block`，`list-item`，`table`的元素。

`Inline level` 的 `box` 会参与形成 `IFC`，比如 `display` 值为 `inline`，`inline-table`，`inline-block` 的元素。

[W3C文档block-level](https://www.w3.org/TR/css3-box/#block-level0)

### FC(Formatting Context)

它是W3C CSS2.1规范中的一个概念，定义的是页面中的一块渲染区域，并且有一套渲染规则，它决定了其子元素将如何定位，以及和其他元素的关系和相互作用。

常见的`Formatting Context` 有：`Block Formatting Context`（`BFC` | 块级格式化上下文） 和 `Inline Formatting Context`（`IFC` |行内格式化上下文）。

 CSS3 新增规范，`GFC`（`GridLayout Formatting Contexts`）以及 `FFC`（`Flex Formatting Context`）。

## BFC

### BFC 布局规则
1. 内部的Box会在垂直方向，一个接一个地放置。
2. Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生重叠
3. 每个元素的左外边缘（margin-left)， 与包含块的左边（contain box left）相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。除非这个元素自己形成了一个新的BFC。
4. BFC的区域不会与float box重叠。
5. BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。
6. 计算BFC的高度时，浮动元素也参与计算

[W3C文档block-formatting](https://www.w3.org/TR/CSS2/visuren.html#block-formatting)

### 怎样形成一个BFC？

1. 根元素或其它包含它的元素
2. 浮动 (元素的 float 不是 none)
3. 绝对定位的元素 (元素具有 position 为 absolute 或 fixed)
4. 非块级元素具有 display: inline-block，table-cell, table-caption, flex, inline-flex
5. 块级元素具有overflow ，且值不是 visible

### BFC用处

1. 清除浮动
2. 布局：自适应两栏布局
3. 防止垂直margin合并

## IFC

### 布局规则
在行内格式化上下文中，框(boxes)一个接一个地水平排列，起点是包含块的顶部。水平方向上的 margin，border 和 padding在框之间得到保留。框在垂直方向上可以以不同的方式对齐：它们的顶部或底部对齐，或根据其中文字的基线对齐。包含那些框的长方形区域，会形成一行，叫做行框。

### 行盒(line box)
1. 包含来自同一行的盒的矩形区域叫做行盒(line box)
2. line box的宽度由包含块和float情况决定,一般来说,line box的宽度等于包含块两边之间的宽度,然而float可以插入到包含块和行盒边之间,如果有float,那么line box的宽度会比没有float时小
3. line box的高度由line-height决定,而line box之间的高度各不相同(比如只含文本的line box高度与包含图片的line box高度之间)
4. line box的高度能够容纳它包含的所有盒,当盒的高度小于行盒的高度(例如,如果盒是baseline对齐)时,盒的竖直对齐方式由vertical-align属性决定
5. 当一行的行内级盒的总宽度小于它们所在的line box的宽度时，它们在行盒里的水平分布由text-align属性决定。如果该属性值为justify，用户代理可能会拉伸行内盒（不包括inline-table和inline-block盒）里的空白和字（间距）

<iframe width="100%" height="300" src="//jsrun.net/DQgKp/embedded/html,css,result/light/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

### 行内盒(inline box)

1. 一个`inline box`是一个（特殊的）行内级盒，其内容参与了它的包含行内格式化上下文
2. 当一个`inline box`超出一个`line box`的宽度时，它会被分成几个盒，并且这些盒会跨多`line box`分布。如果一个`inline-block`无法分割（例如，如果该`inline box`含有一个单个字符，或者特定语言的单词分隔规则不允许在该`inline box`里分隔，或如果该`inline box`受到了一个值为`nowrap`或者`pre`的`white-space`的影响），那么该`inline box`会从`line box`溢出
3. 当一个`inline box`被分割后，`margin，border`和`padding`在发生分割的地方（或者在任何分割处，如果有多处的话）不会有可视化效果
4. 同一个`line box`里的`inline box`也可能因为双向（bidirectional）文本处理而被分割成几个盒

> Line boxes are created as needed to hold inline-level content within an inline formatting context. Line boxes that contain no text, no preserved white space, no inline elements with non-zero margins, padding, or borders, and no other in-flow content (such as images, inline blocks or inline tables), and do not end with a preserved newline must be treated as zero-height line boxes for the purposes of determining the positions of any elements inside of them, and must be treated as not existing for any other purpose.

需要盛放（hold）一个行内格式化上下文中的行内级内容时，创建一个`line box`。不含文本、保留空白符（preserved white space）、`margin`，`padding`或者`border`非0的行内元素、其它流内内容（例如，图片，`inline block`或者`inline table`），并且不以保留换行符（preserved newline）结束的`line box`必须被当作一个0高度的`line box`，为了确定它里面所有元素的位置，而其它时候（for any other purpose）必须当它不存在

[W3C inline-formatting](https://www.w3.org/TR/2011/REC-CSS2-20110607/visuren.html#inline-formatting)

## GFC

GFC(GridLayout Formatting Contexts)直译为"网格布局格式化上下文"，当为一个元素设置display值为grid的时候，此元素将会获得一个独立的渲染区域，我们可以通过在网格容器（grid container）上定义网格定义行（grid definition rows）和网格定义列（grid definition columns）属性各在网格项目（grid item）上定义网格行（grid row）和网格列（grid columns）为每一个网格项目（grid item）定义位置和空间。 

那么GFC有什么用呢，和table又有什么区别呢？首先同样是一个二维的表格，但GridLayout会有更加丰富的属性来控制行列，控制对齐以及更为精细的渲染语义和控制。

## FFC
FFC(Flex Formatting Contexts)直译为"自适应格式化上下文"，display值为flex或者inline-flex的元素将会生成自适应容器（flex container）.

Flex Box 由伸缩容器和伸缩项目组成。通过设置元素的 display 属性为 flex 或 inline-flex 可以得到一个伸缩容器。设置为 flex 的容器被渲染为一个块级元素，而设置为 inline-flex 的容器则渲染为一个行内元素。

伸缩容器中的每一个子元素都是一个伸缩项目。伸缩项目可以是任意数量的。伸缩容器外和伸缩项目内的一切元素都不受影响。简单地说，Flexbox 定义了伸缩容器内伸缩项目该如何布局。

## 参考链接
1. [BFC与IFC概念理解+布局规则+形成方法+用处](https://segmentfault.com/a/1190000009545742)
2. [行内格式化上下文(Inline formatting contexts)](https://segmentfault.com/a/1190000009308818)
3. [\[译\]:BFC与IFC](https://segmentfault.com/a/1190000004466536)
4. [W3C normal flow](https://www.w3.org/TR/2011/REC-CSS2-20110607/visuren.html#normal-flow)
5. [css3之BFC、IFC、GFC和FFC](http://www.cnblogs.com/dingyufenglian/p/4845477.html)

