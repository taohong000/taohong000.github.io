---
title: Vue 2.0 高级实战-开发移动端音乐WebApp 课程笔记（第五章 歌手页面开发）
date: 2017-07-30 16:46:25
categories: 
- 技术
- 课程笔记
tags: 
- 慕课网
- vue
---

# Vue 2.0 高级实战-开发移动端音乐WebApp 课程笔记（第五章 歌手页面开发）

## 歌手数据接口抓取
``` javascript
import jsonp from 'common/js/jsonp'
import {commonParams, options} from './config'

export function getSingerList() {
  const url = 'https://c.y.qq.com/v8/fcg-bin/v8.fcg'

  const data = Object.assign({}, commonParams, {
    channel: 'singer',
    page: 'list',
    key: 'all_all_all',
    pagesize: 100,
    pagenum: 1,
    hostUin: 0,
    needNewCode: 0,
    platform: 'yqq'
  })

  return jsonp(url, data, options)
}
```

## 歌手数据处理和 Singer 类的封装
```javascript
_normalizeSinger(list) {
  let map = {
    hot: {
      title: HOT_NAME,
      items: []
    }
  }

  // 填充hot
  list.forEach((item, index) => {
    if (index < HOT_SINGER_LEN) {
    	// 封装 类Singer
      map.hot.items.push(new Singer({
        name: item.Fsinger_name,
        id: item.Fsinger_mid
      }))
    }

    // 聚合
    const key = item.Findex

    // 创建对象
    if (!map[key]) {
      map[key] = {
        title: key,
        items: []
      }
    }
    map[key].items.push(new Singer({
      name: item.Fsinger_name,
      id: item.Fsinger_mid
    }))
  })
  
  // 为了得到有序列表，我们需要处理 map
  let ret = []
  let hot = []
  for (let key in map) {
    let val = map[key]
    if (val.title.match(/[a-zA-Z]/)) {
      ret.push(val)
    } else if (val.title === HOT_NAME) {
      hot.push(val)
    }
  }
  ret.sort((a, b) => {
    return a.title.charCodeAt(0) - b.title.charCodeAt(0)
  })
  return hot.concat(ret)
}
```
新建common/js/singer.js
```javascript
export default class Singer {
  constructor({id, name}) {
    this.id = id
    this.name = name
    this.avatar = `https://y.gtimg.cn/music/photo_new/T001R300x300M000${id}.jpg?max_age=2592000`
  }
}
```

## listview 基础组件的开发和应用-滚动列表实现
新建通用组件 listview
```javascript
<template>
  <scroll :data="data"
          class="listview">
    <ul>
      <li v-for="group in data" class="list-group" ref="listGroup">
        <h2 class="list-group-title">{{group.title}}</h2>
        <uL>
          <li v-for="item in group.items" class="list-group-item">
            <img class="avatar" v-lazy="item.avatar">
            <span class="name">{{item.name}}</span>
          </li>
        </uL>
      </li>
    </ul>
  </scroll>
</template>

<script>
  import Scroll from 'base/scroll/scroll'

  export default {
    props: {
      data: {
        type: Array,
        default: []
      }
    },
    components: {
      Scroll
    }
  }

</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
  @import "~common/stylus/variable"
  .listview
    position: relative
    width: 100%
    height: 100%
    overflow: hidden
    background: $color-background
    .list-group
      padding-bottom: 30px
      .list-group-title
        height: 30px
        line-height: 30px
        padding-left: 20px
        font-size: $font-size-small
        color: $color-text-l
        background: $color-highlight-background
      .list-group-item
        display: flex
        align-items: center
        padding: 20px 0 0 30px
        .avatar
          width: 50px
          height: 50px
          border-radius: 50%
        .name
          margin-left: 20px
          color: $color-text-l
          font-size: $font-size-medium
    .list-shortcut
      position: absolute
      z-index: 30
      right: 0
      top: 50%
      transform: translateY(-50%)
      width: 20px
      padding: 20px 0
      border-radius: 10px
      text-align: center
      background: $color-background-d
      font-family: Helvetica
      .item
        padding: 3px
        line-height: 1
        color: $color-text-l
        font-size: $font-size-small
        &.current
          color: $color-theme
    .list-fixed
      position: absolute
      top: 0
      left: 0
      width: 100%
      .fixed-title
        height: 30px
        line-height: 30px
        padding-left: 20px
        font-size: $font-size-small
        color: $color-text-l
        background: $color-highlight-background
    .loading-container
      position: absolute
      width: 100%
      top: 50%
      transform: translateY(-50%)
</style>
```

## listview 基础组件的开发和应用-右侧快速入口实现
1. dom.js 添加通用方法
```javascript
export function getData(el, name, val) {
  const prefix = 'data-'
  name = prefix + name
  if (val) {
    return el.setAttribute(name, val)
  }
  return el.getAttribute(name)
}
```
2. scroll.vue 添加方法
``` javascript
scrollTo() {
  this.scroll && this.scroll.scrollTo.apply(this.scroll, arguments)
},
scrollToElement() {
  // 用apply保证调用方法时上下文相同
  this.scroll && this.scroll.scrollToElement.apply(this.scroll, arguments)
}
```
3. viewlist.vue添加dom
```html
<div class="list-shortcut"
 @touchstart.stop.prevent="onShortcutTouchStart" 
 @touchmove.stop.prevent="onShortcutTouchMove"
 @touchend.stop>
  <ul>
    <li v-for="(item, index) in shortcutList" :data-index="index" class="item">{{item}}
    </li>
  </ul>
</div>
```
4. viewlist.vue添加方法
```javascript
onShortcutTouchStart(e) {
  let anchorIndex = getData(e.target, 'index')
  let firstTouch = e.touches[0]
  this.touch.y1 = firstTouch.pageY
  this.touch.anchorIndex = anchorIndex
  this.$refs.listview.scrollToElement(this.$refs.listGroup[anchorIndex], 0)
  this._scrollTo(anchorIndex)
},
onShortcutTouchMove(e) {
  let firstTouch = e.touches[0]
  this.touch.y2 = firstTouch.pageY
  let delta = (this.touch.y2 - this.touch.y1) / ANCHOR_HEIGHT | 0
  let anchorIndex = parseInt(this.touch.anchorIndex) + delta

  this._scrollTo(anchorIndex)
},
_scrollTo(index) {
  this.$refs.listview.scrollToElement(this.$refs.listGroup[index], 0)
}
```

5. 计算所有group的高度
```javascript
_calculateHeight() {
  this.listHeight = []
  const list = this.$refs.listGroup
  let height = 0
  this.listHeight.push(height)
  for (let i = 0; i < list.length; i++) {
    let item = list[i]
    height += item.clientHeight
    this.listHeight.push(height)
  }
}
```

6 添加watch 计算 currentIndex
```javascript
watch: {
 data() {
    setTimeout(() => {
      // 数据变化到dom的变化有一个延时
      this._calculateHeight()
    }, 20)
  },
  scrollY(newY) {
    const listHeight = this.listHeight
    // 当滚动到顶部，newY>0
    if (newY > 0) {
      this.currentIndex = 0
      return
    }
    // 在中间部分滚动
    for (let i = 0; i < listHeight.length - 1; i++) {
      let height1 = listHeight[i]
      let height2 = listHeight[i + 1]
      if (-newY >= height1 && -newY < height2) {
        this.currentIndex = i
        this.diff = height2 + newY
        return
      }
    }
    // 当滚动到底部，且-newY大于最后一个元素的上限
    this.currentIndex = listHeight.length - 2
  }
}
```

总结:联动思路
1. 知道实时滚动位置
2. 根据滚动位置计算落在哪个group区间
3. 根据区间计算索引哪个高亮

## listview 基础组件的开发和应用-滚动固定标题实现
1. 添加dom
```html
<div class="list-fixed" ref="fixed" v-show="fixedTitle">
 <div class="fixed-title">{{fixedTitle}} </div>
</div>
```

2. 添加计算属性
```javascript
fixedTitle() {
  if (this.scrollY > 0) {
    return ''
  }
  return this.data[this.currentIndex] ? this.data[this.currentIndex].title : ''
}
```

3. 添加watch
```javascript
diff(newVal) {
  let fixedTop = (newVal > 0 && newVal < TITLE_HEIGHT) ? newVal - TITLE_HEIGHT : 0
  // 减少dom操作的频度
  if (this.fixedTop === fixedTop) {
    return
  }
  this.fixedTop = fixedTop
  this.$refs.fixed.style.transform = `translate3d(0,${fixedTop}px,0)`
}
```
