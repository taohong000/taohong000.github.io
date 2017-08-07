---
title: Vue 2.0 高级实战-开发移动端音乐WebApp 课程笔记（第八章 歌单页面开发）
date: 2017-08-07 10:42:56
categories: 
- 技术
- 课程笔记
tags: 
- 慕课网
- vue
---
# Vue 2.0 高级实战-开发移动端音乐WebApp 课程笔记（第八章 歌单页面开发）

## 歌单详情页布局介绍及Vuex实现路由数据通讯

## 歌单详情页数据抓取
api/recommend.js 添加方法
```javascript
export function getSongList(disstid) {
  const url = 'https://c.y.qq.com/qzone/fcg-bin/fcg_ucc_getcdinfo_byids_cp.fcg'

  const data = Object.assign({}, commonParams, {
    disstid,
    type: 1,
    json: 1,
    utf8: 1,
    onlysong: 0,
    platform: 'yqq',
    hostUin: 0,
    needNewCode: 0
  })

  return jsonp(url, data, options)
}
```

## 歌单详情页数据的处理和应用
```javascript
_normalizeSongs(list) {
  let ret = []
  list.forEach((musicData) => {
    if (musicData.songid && musicData.albummid) {
      ret.push(createSong(musicData))
    }
  })
  return ret
}
```