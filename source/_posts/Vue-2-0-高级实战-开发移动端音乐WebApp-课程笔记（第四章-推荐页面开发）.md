---
title: Vue 2.0 高级实战-开发移动端音乐WebApp 课程笔记（第四章 推荐页面开发）
date: 2017-07-26 23:24:16
categories: 
- 技术
- 课程笔记
tags: 
- 慕课网
- vue
---

# Vue 2.0 高级实战-开发移动端音乐WebApp 课程笔记（第四章 推荐页面开发）

## jsonp原理介绍+Promise封装

### jsonp
[jsonp github库](https://github.com/webmodules/jsonp)

### Promise封装

```javascript
import originJsonp from 'jsonp'

export default function jsonp(url, data, option) {
  url += (url.indexOf('?') < 0 ? '?' : '&') + param(data)

  return new Promise((resolve, reject) => {
    originJsonp(url, option, (err, data) => {
      if (!err) {
        resolve(data)
      } else {
        reject(err)
      }
    })
  })
}

export function param(data) {
  let url = ''
  for (var k in data) {
    let value = data[k] !== undefined ? data[k] : ''
    url += '&' + k + '=' + encodeURIComponent(value)
  }
  return url ? url.substring(1) : ''
}
```

## jsonp的应用+轮播图数据抓取

1. 添加请求通用配置文件src/api/config.js
```javascript
export const commonParams = {
  g_tk: 5381,
  inCharset: 'utf-8',
  outCharset: 'utf-8',
  notice: 0,
  format: 'jsonp'
}

export const options = {
  param: 'jsonpCallback'
}

export const ERR_OK = 0
```

2. 添加接口文件src/api/recommend.js
```javascript
import jsonp from 'common/js/jsonp'
import {commonParams, options} from './config'

export function getRecommend() {
  const url = 'https://c.y.qq.com/musichall/fcgi-bin/fcg_yqqhomepagerecommend.fcg'

  const data = Object.assign({}, commonParams, {
    platform: 'h5',
    uin: 0,
    needNewCode: 1
  })

  return jsonp(url, data, options)
}
```

3. 创建 components/recommend/recommend.vue组件
```html
<template>
  <div class="recommend" ref="recommend">
  </div>
</template>

<script>
  import {getRecommend} from 'api/recommend'
  import {ERR_OK} from 'api/config'

  export default {
    data() {
      return {
        recommends: []
      }
    },
    created() {
      this._getRecommend()
    },
    methods: {
      _getRecommend() {
        getRecommend().then((res) => {
          if (res.code === ERR_OK) {
            this.recommends = res.data.slider
          }
        })
      }
    }
  }
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
  @import "~common/stylus/variable"
  .recommend
    position: fixed
    width: 100%
    top: 88px
    bottom: 0
    .recommend-content
      height: 100%
      overflow: hidden
      .slider-wrapper
        position: relative
        width: 100%
        overflow: hidden
      .recommend-list
        .list-title
          height: 65px
          line-height: 65px
          text-align: center
          font-size: $font-size-medium
          color: $color-theme
        .item
          display: flex
          box-sizing: border-box
          align-items: center
          padding: 0 20px 20px 20px
          .icon
            flex: 0 0 60px
            width: 60px
            padding-right: 20px
          .text
            display: flex
            flex-direction: column
            justify-content: center
            flex: 1
            line-height: 20px
            overflow: hidden
            font-size: $font-size-medium
            .name
              margin-bottom: 10px
              color: $color-text
            .desc
              color: $color-text-d
      .loading-container
        position: absolute
        width: 100%
        top: 50%
        transform: translateY(-50%)
</style>
```

##  轮播图组件实现
1. 创建基础组件 base/slider/slider.vue
``` html 
<template>
  <div class="slider" ref="slider">
    <div class="slider-group" ref="sliderGroup">
      <slot>
      </slot>
    </div>
    <div class="dots">
      <span class="dot" :class="{active: currentPageIndex === index }" v-for="(item, index) in dots"></span>
    </div>
  </div>
</template>

<script>
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
  @import "~common/stylus/variable"
  .slider
    min-height: 1px
    .slider-group
      position: relative
      overflow: hidden
      white-space: nowrap
      .slider-item
        float: left
        box-sizing: border-box
        overflow: hidden
        text-align: center
        a
          display: block
          width: 100%
          overflow: hidden
          text-decoration: none
        img
          display: block
          width: 100%
    .dots
      position: absolute
      right: 0
      left: 0
      bottom: 12px
      text-align: center
      font-size: 0
      .dot
        display: inline-block
        margin: 0 4px
        width: 8px
        height: 8px
        border-radius: 50%
        background: $color-text-l
        &.active
          width: 20px
          border-radius: 5px
          background: $color-text-ll
</style>
```
 
 2. 添加设置
```javascript
export default {
  name: 'slider',
  props: {
    // 循环轮播
    loop: {
      type: Boolean,
      default: true
    },
    // 自动录播
    autoPlay: {
      type: Boolean,
      default: true
    },
    // 轮播间隔
    interval: {
      type: Number,
      default: 4000
    }
  }
}
```

3. 引入better-scroll并设置
``` javascript
// 设置sliderGroup宽度
_setSliderWidth(isResize) {
        this.children = this.$refs.sliderGroup.children

        let width = 0
        let sliderWidth = this.$refs.slider.clientWidth
        for (let i = 0; i < this.children.length; i++) {
          let child = this.children[i]
          addClass(child, 'slider-item')

          child.style.width = sliderWidth + 'px'
          width += sliderWidth
        }
        if (this.loop && !isResize) {
          width += 2 * sliderWidth
        }
        this.$refs.sliderGroup.style.width = width + 'px'
}
```
4. 添加common/js/dom.js
```javascript
export function hasClass(el, className) {
  let reg = new RegExp('(^|\\s)' + className + '(\\s|$)')
  return reg.test(el.className)
}

export function addClass(el, className) {
  if (hasClass(el, className)) {
    return
  }

  let newClass = el.className.split(' ')
  newClass.push(className)
  el.className = newClass.join(' ')
}
```

5. 在mounted中添加方法
```javascript
// 一般网页刷新为17毫秒
  setTimeout(() => {
     this._setSliderWidth()
     if (this.autoPlay) {
       this._play()
     }
   }, 20)
```

6. 在recommend.vue中使用组件
```javascript
// v-if确保slider中有元素后再执行
<div v-if="recommends.length" class="slider-wrapper" ref="sliderWrapper">
  <slider>
    <div v-for="item in recommends">
      <a :href="item.linkUrl">
        <img class="needsclick" @load="loadImage" :src="item.picUrl">
      </a>
    </div>
  </slider>
</div>
```

6. 在slider.vue中初始化slider
```javascript
_initSlider() {
  this.slider = new BScroll(this.$refs.slider, {
    scrollX: true,
    scrollY: false,
    momentum: false,
    snap: true,
    snapLoop: this.loop,
    snapThreshold: 0.3,
    snapSpeed: 400
  })
}
```

7. 初始化dots
```javascript
// 添加methods
_initDots() {
  this.dots = new Array(this.children.length)
}

// 在_initSlider中添加监听事件
this.slider.on('scrollEnd', () => {
  let pageIndex = this.slider.getCurrentPage().pageX
  if (this.loop) {
    pageIndex -= 1
  }
  this.currentPageIndex = pageIndex

  if (this.autoPlay) {
    this._play()
  }
})
```

8. 添加自动播放事件
```javascript
// 添加methods
_initDots() {
  this.dots = new Array(this.children.length)
}

// 在_initSlider中添加监听事件
_play() {
  let pageIndex = this.currentPageIndex + 1
  if (this.loop) {
    pageIndex += 1
  }
  this.timer = setTimeout(() => {
    this.slider.goToPage(pageIndex, 0, 400)
  }, this.interval)
}
```

9. 修改_initSlider
```javascript
_initSlider() {
  this.slider = new BScroll(this.$refs.slider, {
      scrollX: true,
      scrollY: false,
      momentum: false,
      snap: true,
      snapLoop: this.loop,
      snapThreshold: 0.3,
      snapSpeed: 400,
      click: true
  })

  this.slider.on('scrollEnd', () => {
      let pageIndex = this.slider.getCurrentPage().pageX
      if (this.loop) {
          pageIndex -= 1
      }
      this.currentPageIndex = pageIndex
      // 添加自动播放
      if (this.autoPlay) {
          this._play()
      }
  })
  // 手动滑动后清除定时器
  this.slider.on('beforeScrollStart', () => {
      if (this.autoPlay) {
          clearTimeout(this.timer)
      }
  })
}
```
10. 添加窗口改变监听事件
```javascript
window.addEventListener('resize', () => {
  if (!this.slider) {
    return
  }
  this._setSliderWidth(true)
  this.slider.refresh()
})
```

## 歌单数据接口, axios 介绍和后端接口代理
1. 在dev-server
```javascript
var apiRoutes = express.Router()

apiRoutes.get('/getDiscList', function (req, res) {
  var url = 'https://c.y.qq.com/splcloud/fcgi-bin/fcg_get_diss_by_tag.fcg'
  axios.get(url, {
    headers: {
      referer: 'https://c.y.qq.com/',
      host: 'c.y.qq.com'
    },
    params: req.query
  }).then((response) => {
    res.json(response.data)
  }).catch((e) => {
    console.log(e)
  })
})

app.use('/api', apiRoutes)
```
2. 在src/api/recommend.js中添加接口getDiscList
```javascript
export function getDiscList() {
  const url = '/api/getDiscList'

  const data = Object.assign({}, commonParams, {
    platform: 'yqq',
    hostUin: 0,
    sin: 0,
    ein: 29,
    sortId: 5,
    needNewCode: 0,
    categoryId: 10000000,
    rnd: Math.random(),
    format: 'json'
  })

  return axios.get(url, {
    params: data
  }).then((res) => {
    return Promise.resolve(res.data)
  })
}
```
2. 在recommend.vue中添加列表
```html
<div class="recommend-list">
 <h1 class="list-title">热门歌单推荐</h1>
  <ul>
    <li v-for="item in discList" class="item">
      <div class="icon">
        <img width="60" height="60"  :src="item.imgurl">
      </div>
      <div class="text">
      // v-html:转义
        <h2 class="name" v-html="item.creator.name"></h2>
        <p class="desc" v-html="item.dissname"></p>
      </div>
    </li>
  </ul>
</div>
```

## scroll 组件的抽象和应用
1. 添加通用组件scroll
```html
<template>
  <div ref="wrapper">
    <slot></slot>
  </div>
</template>

<script>
  import BScroll from 'better-scroll'

  export default {
    props: {
      probeType: {
        type: Number,
        default: 1
      },
      click: {
        type: Boolean,
        default: true
      },
      data: {
        type: Array,
        default: null
      }
    },
    mounted() {
      setTimeout(() => {
        this._initScroll()
      }, 20)
    },
    methods: {
      _initScroll() {
        if (!this.$refs.wrapper) {
          return
        }
        this.scroll = new BScroll(this.$refs.wrapper, {
          probeType: this.probeType,
          click: this.click
        })
      },
      enable() {
        this.scroll && this.scroll.enable()
      },
      disable() {
        this.scroll && this.scroll.disable()
      },
      refresh() {
        this.scroll && this.scroll.refresh()
      }
    },
    watch: {
      data() {
        setTimeout(() => {
          this.refresh()
        }, 20)
      }
    }
  }

</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
</style>
```
2. recommend.vue中添加loadImage
```javascript
loadImage() {
  // this.checkloaded:标志位, 只计算一次
  if (!this.checkloaded) {
    this.checkloaded = true
    this.$refs.scroll.refresh()
  }
},
```
## vue-lazyload[vue-lazyload](https://github.com/hilongjw/vue-lazyload) 懒加载插件介绍和应用
1. main.js
```javascript
import VueLazyload from 'vue-lazyload'
Vue.use(VueLazyload, {
  loading: require('common/image/default.png')
})
```

2. recommend.vue
```html
<img width="60" height="60"  v-lazy="item.imgurl">
```
3. fastclick避免拦截, 加class needsclick
```html
<img class="needsclick" @load="loadImage" :src="item.picUrl">
```

## loading 基础组件的开发和应用

1. 添加基础组件loading.vue 在 recommend.vue中使用
```html
<template>
  <div class="loading">
    <img width="24" height="24" src="./loading.gif">
    <p class="desc">{{title}}</p>
  </div>
</template>
<script>
  export default {
    props: {
      title: {
        type: String,
        default: '正在载入...'
      }
    }
  }
</script>
<style scoped lang="stylus" rel="stylesheet/stylus">
  @import "~common/stylus/variable"
  .loading
    width: 100%
    text-align: center
    .desc
      line-height: 20px
      font-size: $font-size-small
      color: $color-text-l
</style>
```