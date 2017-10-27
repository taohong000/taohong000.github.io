---
title: Vue 2.0 高级实战-开发移动端音乐WebApp 课程笔记（第九章 排行榜及详情页开发）
date: 2017-08-10 15:17:29
categories: 
- 技术
- 课程笔记
tags: 
- 慕课网
- vue
---

# Vue 2.0 高级实战-开发移动端音乐WebApp 课程笔记（第九章 排行榜及详情页开发）


## 排行页面布局介绍及排行榜数据抓取
添加api/rank.js
```javascript
import jsonp from 'common/js/jsonp'
import {commonParams, options} from './config'

export function getTopList() {
  const url = 'https://c.y.qq.com/v8/fcg-bin/fcg_myqq_toplist.fcg'

  const data = Object.assign({}, commonParams, {
    needNewCode: 1,
    platform: 'h5'
  })

  return jsonp(url, data, options)
}

```

## 排行页排行榜数据应用
新建组件rank

## 榜单详情页布局介绍及Vuex实现路由数据通讯
新建组件top-list

## 榜单详情页数据抓取和应用
```javascript
export function getMusicList(topid) {
  const url = 'https://c.y.qq.com/v8/fcg-bin/fcg_v8_toplist_cp.fcg'

  const data = Object.assign({}, commonParams, {
    topid,
    needNewCode: 1,
    uin: 0,
    tpl: 3,
    page: 'detail',
    type: 'top',
    platform: 'h5'
  })

  return jsonp(url, data, options)
}
```

## 带排行的song-list组件扩展和应用
1. 添加dom
```html
<div class="rank" v-show="rank">
  <span :class="getRankCls(index)">{{getRankText(index)}}</span>
</div>
```

2. 添加props
```javascript
rank: {
  type: Boolean,
  default: false
}
```

3. 添加methods
```javascript
getRankCls(index) {
  if (index <= 2) {
    return `icon icon${index}`
  } else {
    return 'text'
  }
},
getRankText(index) {
  if (index > 2) {
    return index + 1
  }
}
```

4. music-list添加props
```javascript
rank: {
  type: Boolean,
  default: false
}
```
