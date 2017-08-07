---
title: 添加hexo Next主题自定样式
date: 2017-08-07 17:23:09
tags:
- hexo
- Next
---

# 添加hexo Next主题自定样式

添加自定义样式主要有两种
1. 添加自定义css
2. 添加自定义内置标签

## 添加自定义样式
在themes/next/source/css/_custom/中添加样式文件, 例如
```css
.label{
    font-family: Consolas;
    display: inline;
    color: #fff;
    padding: 1px 3px 2px 3px;
    margin: 0px 2px 0px 2px;
    border-radius: 2px;
}
.label-red{
    background: #d9534f;
}
.label-green{
    background: #5cb85c;
}
.label-blue{
    background: #428bca;
}
.label-sky{
    background: #5bc0de;
}
.label-orange{
    background: #f0ad4e;
}
```

在themes/next/source/css/main.styl用引入样式文件
```stylus
@import "_custom/custom";
```

在文章中使用
```html
<span class="label label-red">New</span>
```

## 添加自定义内置标签
在themes/next/scripts/tags中添加文件, 例如
```javascript
function bscallOut (args, content) {
  return '<div class="note ' + args.join(' ') + '">' + hexo.render.renderSync({text: content, engine: 'markdown'}).trim() + '</div>';
}

hexo.extend.tag.register('note', bscallOut, {ends: true});
```
在文章中使用
```
{% note [class] %}
  Any content (support inline tags too).
{% endnote %}

// [class] : default | primary | success | info | warning | danger
```

[ hexo完整的标签列表](https://hexo.io/docs/tag-plugins.html)

Next主题内置的[Bootstrap Callout](https://almostover.ru/2016-01/hexo-theme-next-test/)
