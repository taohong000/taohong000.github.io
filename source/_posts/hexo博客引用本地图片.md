---
title: hexo博客引用本地图片
date: 2017-11-01 15:29:31
categories: 
- 技术
tags: 
- hexo
---

## 使用插件

1. 设置 _config.yml
``` javascript
post_asset_folder:true

```
2. 安装插件
``` bash
npm install https://github.com/CodeFalling/hexo-asset-image --save
```
3. 引入图片
``` bash
![avatar](图片目录/logo.jpg)
```

![avatar](hexo博客引用本地图片/avatar.jpg)

## 使用标签
1. 设置 _config.yml
``` javascript
post_asset_folder:true

```
2. 使用标签
``` bash
{% asset_path slug %}
{% asset_img slug [title] %}
{% asset_link slug [title] %}

如:
{% asset_img avatar.jpg avatar %}
```

{% asset_img avatar.jpg avatar %}

