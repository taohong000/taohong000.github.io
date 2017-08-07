---
title: Vue 2.0 高级实战-开发移动端音乐WebApp 课程笔记（第七章 播放器内置组件开发）
date: 2017-08-07 10:40:32
categories: 
- 技术
- 课程笔记
tags: 
- 慕课网
- vue
---

# Vue 2.0 高级实战-开发移动端音乐WebApp 课程笔记（第七章 播放器内置组件开发）

## 播放器Vuex数据设计
1. state
```javascript
const state = {
  singer: {},
  playing: false,
  fullScreen: false,
  playlist: [],
  sequenceList: [],
  mode: playMode.sequence,
  currentIndex: -1
}
```
state只保留最基础的数据, 计算属性放在getters中

2. getters
```javascript
export const playing = state => state.playing

export const fullScreen = state => state.fullScreen

export const playlist = state => state.playlist

export const sequenceList = state => state.sequenceList

export const mode = state => state.mode

export const currentIndex = state => state.currentIndex

export const currentSong = (state) => {
  return state.playlist[state.currentIndex] || {}
}
```

3. mutation-types
```javascript
export const SET_PLAYING_STATE = 'SET_PLAYING_STATE'

export const SET_FULL_SCREEN = 'SET_FULL_SCREEN'

export const SET_PLAYLIST = 'SET_PLAYLIST'

export const SET_SEQUENCE_LIST = 'SET_SEQUENCE_LIST'

export const SET_PLAY_MODE = 'SET_PLAY_MODE'

export const SET_CURRENT_INDEX = 'SET_CURRENT_INDEX'
```

4. mutations
```javascript
import * as types from './mutation-types'

const matutaions = {
  [types.SET_SINGER](state, singer) {
    state.singer = singer
  },
  [types.SET_PLAYING_STATE](state, flag) {
    state.playing = flag
  },
  [types.SET_FULL_SCREEN](state, flag) {
    state.fullScreen = flag
  },
  [types.SET_PLAYLIST](state, list) {
    state.playlist = list
  },
  [types.SET_SEQUENCE_LIST](state, list) {
    state.sequenceList = list
  },
  [types.SET_PLAY_MODE](state, mode) {
    state.mode = mode
  },
  [types.SET_CURRENT_INDEX](state, index) {
    state.currentIndex = index
  }
}

export default matutaions
```

actions的操作通常有两种
1. 异步操作
2. 对mutation的封装

## 播放器Vuex的相关应用
1. 新建组件player.vue
```html 
<template>
  <div class="player">
    <div class="normal-player">
      播放器
    </div>
    <div class="mini-player"></div>
  </div>
</template>

<script>
  export default {}
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">

</style>
```

2. 在App.vue中注册组件

3. 在song-list.vue 中派发事件
```javascript
selectItem(song, index) {
  this.$emit('select', item, index)
}
```

4. 在actions定义动作
```javascript
import * as types from './mutation-types'

export const selectPlay = function ({commit, state}, {list, index}) {
  commit(types.SET_SEQUENCE_LIST, list)
  commit(types.SET_PLAYLIST, randomList)
  commit(types.SET_CURRENT_INDEX, index)
  commit(types.SET_FULL_SCREEN, true)
  commit(types.SET_PLAYING_STATE, true)
}
```

5. 在music-list.vue 接收事件
```javascript
selectItem(item, index) {
  this.selectPlay({
    list: this.songs,
    index
  })
},
...mapActions([
  'selectPlay'
])
```

## 播放器基础样式及歌曲数据的应用
```html
<template>
  <div class="music-list">
    <div class="back" @click="back">
      <i class="icon-back"></i>
    </div>
    <h1 class="title" v-html="title"></h1>
    <div class="bg-image" :style="bgStyle" ref="bgImage">
      <div class="play-wrapper">
        <div ref="playBtn" v-show="songs.length>0" class="play">
          <i class="icon-play"></i>
          <span class="text">随机播放全部</span>
        </div>
      </div>
      <div class="filter" ref="filter"></div>
    </div>
    <div class="bg-layer" ref="layer"></div>
    <scroll :data="songs" @scroll="scroll" :listen-scroll="listenScroll" :probe-type="probeType" class="list" ref="list">
      <div class="song-list-wrapper">
        <song-list @select="selectItem" :songs="songs"></song-list>
      </div>
      <div v-show="!songs.length" class="loading-container">
        <loading></loading>
      </div>
    </scroll>
  </div>
</template>
<script>
import Scroll from 'base/scroll/scroll'
import Loading from 'base/loading/loading'
import SongList from 'base/song-list/song-list'
import { prefixStyle } from 'common/js/dom'
import { mapActions } from 'vuex'

const RESERVED_HEIGHT = 40
const transform = prefixStyle('transform')
const backdrop = prefixStyle('backdrop-filter')

export default {
  props: {
    bgImage: {
      type: String,
      default: ''
    },
    songs: {
      type: Array,
      default: []
    },
    title: {
      type: String,
      default: ''
    }
  },
  data() {
    return {
      scrollY: 0
    }
  },
  computed: {
    bgStyle() {
      return `background-image:url(${this.bgImage})`
    }
  },
  created() {
    this.probeType = 3
    this.listenScroll = true
  },
  mounted() {
    this.imageHeight = this.$refs.bgImage.clientHeight
    this.minTransalteY = -this.imageHeight + RESERVED_HEIGHT
    this.$refs.list.$el.style.top = `${this.imageHeight}px`
  },
  methods: {
    scroll(pos) {
      this.scrollY = pos.y
    },
    back() {
      this.$router.back()
    },
    // 子组件行为和本身相关, 不要依赖外部组件使用定义子组件, 子组件行为和本身相关, 传item值是子组件点击时该做的事
    selectItem(item, index) {
      this.selectPlay({
        list: this.songs,
        index
      })
    },
    ...mapActions([
      'selectPlay'
    ])
  },
  watch: {
    scrollY(newVal) {
      let translateY = Math.max(this.minTransalteY, newVal)
      let scale = 1
      let zIndex = 0
      let blur = 0
      // 计算图片放大比例
      const percent = Math.abs(newVal / this.imageHeight)
      if (newVal > 0) {
        scale = 1 + percent
        zIndex = 10
      } else {
        // 计算模糊比例
        blur = Math.min(20, percent * 20)
      }

      this.$refs.layer.style[transform] = `translate3d(0,${translateY}px,0)`
      this.$refs.filter.style[backdrop] = `blur(${blur}px)`
      if (newVal < this.minTransalteY) {
        // 向上滑动超过minTransalteY, 改变图片zIndex和高度, 按钮消失
        zIndex = 10
        this.$refs.bgImage.style.paddingTop = 0
        this.$refs.bgImage.style.height = `${RESERVED_HEIGHT}px`
        this.$refs.playBtn.style.display = 'none'
      } else {
        // 反之, 重置
        this.$refs.bgImage.style.paddingTop = '70%'
        this.$refs.bgImage.style.height = 0
        this.$refs.playBtn.style.display = ''
      }
      this.$refs.bgImage.style[transform] = `scale(${scale})`
      this.$refs.bgImage.style.zIndex = zIndex
    }
  },
  components: {
    Scroll,
    Loading,
    SongList
  }
}
```

## 播放器展开收起动画
第三方库: [create-keyframe-animation](https://github.com/HenrikJoreteg/create-keyframe-animation)
```html
<transition name="normal"
                @enter="enter"
                @after-enter="afterEnter"
                @leave="leave"
                @after-leave="afterLeave">
  ...normal
</transition>
<script>              
```
```javascript
// 导入插件
import animations from 'create-keyframe-animation'
import {prefixStyle} from 'common/js/dom'

const transform = prefixStyle('transform')

// 添加methods方法
/**
 * 动画进入
 * @param  {object}   el   dom
 * @param  {Function} done 回调函数, 即afterEnter
 */
enter(el, done) {
  const {x, y, scale} = this._getPosAndScale()

  let animation = {
    0: {
      transform: `translate3d(${x}px,${y}px,0) scale(${scale})`
    },
    60: {
      transform: `translate3d(0,0,0) scale(1.1)`
    },
    100: {
      transform: `translate3d(0,0,0) scale(1)`
    }
  }

  animations.registerAnimation({
    name: 'move',
    animation,
    presets: {
      duration: 400,
      easing: 'linear'
    }
  })

  animations.runAnimation(this.$refs.cdWrapper, 'move', done)
},
/**
 * 注销动画
 */
afterEnter() {
  animations.unregisterAnimation('move')
  this.$refs.cdWrapper.style.animation = ''
},
/**
 * 动画离开
 * @param  {object}   el   dom
 * @param  {Function} done 回调函数, 即afterLeave
 */
leave(el, done) {
  this.$refs.cdWrapper.style.transition = 'all 0.4s'
  const {x, y, scale} = this._getPosAndScale()
  this.$refs.cdWrapper.style[transform] = `translate3d(${x}px,${y}px,0) scale(${scale})`
  this.$refs.cdWrapper.addEventListener('transitionend', done)
},
afterLeave() {
  this.$refs.cdWrapper.style.transition = ''
  this.$refs.cdWrapper.style[transform] = ''
},
/**
 * 计算相对位置和缩放比例
 * @return {object} 宽度, 高度, 缩放比例
 */
_getPosAndScale() {
  const targetWidth = 40
  const paddingLeft = 40
  const paddingBottom = 30
  const paddingTop = 80
  const width = window.innerWidth * 0.8
  const scale = targetWidth / width
  const x = -(window.innerWidth / 2 - paddingLeft)
  const y = window.innerHeight - paddingTop - width / 2 - paddingBottom
  return {
    x,
    y,
    scale
  }
},
```

## 播放器歌曲播放功能实现
1. 添加radio
```html
<audio ref="audio" :src="currentSong.url"></audio>
```

2. 添加methods: togglePlaying, watch: currentSong,
playing
```javascript
// methods
togglePlaying() {
  this.setPlayingState(!this.playing)
}

// watch
currentSong(newSong, oldSong) {
  this.$nextTick(() => {
    this.$refs.audio.play()
  })
},
playing(newPlaying) {
  const audio = this.$refs.audio
  this.$nextTick(() => {
    newPlaying ? audio.play() : audio.pause()
  })
}
```
3. 添加计算属性, 计算图标
``` javascript
playIcon() {
  return this.playing ? 'icon-pause' : 'icon-play'
},
miniIcon() {
  return this.playing ? 'icon-pause-mini' : 'icon-play-mini'
}
```

4. 添加计算属性, 计算图标是否旋转
```javascript
cdCls() {
  return this.playing ? 'play' : 'play pause'
}
```
```scss
.cd {
	&.play {
		animation: rotate 20s linear infinite
	}
	&.pause {
		animation-play-state: paused
	}
}

@keyframes rotate {
	0% {
		transform: rotate(0)
	}
    100% {
	    transform: rotate(360deg)
    }
}
```

## 播放器歌曲前进后退功能实现
1. 添加歌曲上一首, 下一首事件
``` javascript
// 播放下一首歌
next() {
  if (!this.songReady) {
    return
  }
  let index = this.currentIndex + 1
  if (index === this.playlist.length) {
    index = 0
  }
  this.setCurrentIndex(index)
  if (!this.playing) {
    this.togglePlaying()
  }
  this.songReady = false
},
// 播放上一首歌
prev() {
  if (!this.songReady) {
    return
  }
  let index = this.currentIndex - 1
  if (index === -1) {
    index = this.playlist.length - 1
  }
  this.setCurrentIndex(index)
  if (!this.playing) {
    this.togglePlaying()
  }
  this.songReady = false
},
ready() {
  this.songReady = true
}
```

## 播放器播放时间获取和更新
添加事件
```javascript
// 获取播放时间
updateTime(e) {
  this.currentTime = e.target.currentTime
}

// 补位
_pad(num, n = 2) {
  let len = num.toString().length
  while (len < n) {
    num = '0' + num
    len++
  }
  return num
},
// 格式化
format(interval) {
  interval = interval | 0
  const minute = interval / 60 | 0
  const second = this._pad(interval % 60)
  return `${minute}:${second}`
}
```

## 播放器progress-bar进度条组件实现
1. palyer.vue 添加计算属性
```javascript
percent() {
  return this.currentTime / this.currentSong.duration
}
```

2. 添加基础组件progress-bar.vue
```html
<template>
  <div class="progress-bar" ref="progressBar" @click="progressClick">
    <div class="bar-inner">
      <div class="progress" ref="progress"></div>
      <div class="progress-btn-wrapper" ref="progressBtn"
           @touchstart.prevent="progressTouchStart"
           @touchmove.prevent="progressTouchMove"
           @touchend="progressTouchEnd"
      >
        <div class="progress-btn"></div>
      </div>
    </div>
  </div>
</template>

<script>
  import {prefixStyle} from 'common/js/dom'

  const progressBtnWidth = 16
  const transform = prefixStyle('transform')

  export default {
    props: {
      percent: {
        type: Number,
        default: 0
      }
    },
    created() {
      this.touch = {}
    },
    methods: {
      progressTouchStart(e) {
        // 标识位, 表示已经初始化
        this.touch.initiated = true
        // 点击位置
        this.touch.startX = e.touches[0].pageX
        // 进度条宽度
        this.touch.left = this.$refs.progress.clientWidth
      },
      progressTouchMove(e) {
        if (!this.touch.initiated) {
          return
        }
        // 偏移量
        const deltaX = e.touches[0].pageX - this.touch.startX
        // 移动距离
        const offsetWidth = Math.min(this.$refs.progressBar.clientWidth - progressBtnWidth, Math.max(0, this.touch.left + deltaX))
        this._offset(offsetWidth)
      },
      progressTouchEnd() {
        this.touch.initiated = false
        this._triggerPercent()
      },
      /**
       * 点击进度条事件
       * @param  {Object} e event对象
       */
      progressClick(e) {
        const rect = this.$refs.progressBar.getBoundingClientRect()
        const offsetWidth = e.pageX - rect.left
        this._offset(offsetWidth)
        // 这里当我们点击 progressBtn 的时候，e.offsetX 获取不对
        // this._offset(e.offsetX)
        this._triggerPercent()
      },
      _triggerPercent() {
        const barWidth = this.$refs.progressBar.clientWidth - progressBtnWidth
        const percent = this.$refs.progress.clientWidth / barWidth
        this.$emit('percentChange', percent)
      },
      _offset(offsetWidth) {
        this.$refs.progress.style.width = `${offsetWidth}px`
        this.$refs.progressBtn.style[transform] = `translate3d(${offsetWidth}px,0,0)`
      }
    },
    watch: {
      percent(newPercent) {
        // 没有在拖动中
        if (newPercent >= 0 && !this.touch.initiated) {
          // 进度条宽度
          const barWidth = this.$refs.progressBar.clientWidth - progressBtnWidth
          // 偏移宽度
          const offsetWidth = newPercent * barWidth
          this._offset(offsetWidth)
        }
      }
    }
  }
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
  @import "~common/stylus/variable"
  .progress-bar
    height: 30px
    .bar-inner
      position: relative
      top: 13px
      height: 4px
      background: rgba(0, 0, 0, 0.3)
      .progress
        position: absolute
        height: 100%
        background: $color-theme
      .progress-btn-wrapper
        position: absolute
        left: -8px
        top: -13px
        width: 30px
        height: 30px
        .progress-btn
          position: relative
          top: 7px
          left: 7px
          box-sizing: border-box
          width: 16px
          height: 16px
          border: 3px solid $color-text
          border-radius: 50%
          background: $color-theme
</style>
```
3. 组件player.vue 添加事件
```javascript
onProgressBarChange(percent) {
  const currentTime = this.currentSong.duration * percent
  this.$refs.audio.currentTime = currentTime
  if (!this.playing) {
    this.togglePlaying()
  }
},
```

# 播放器progress-circle 圆形进度条组件实现
添加基础组件progress-circle
```html
<template>
  <div class="progress-circle">
	<!-- viewBox和r成比例 -->  
    <svg :width="radius" :height="radius" viewBox="0 0 100 100" version="1.1" xmlns="http://www.w3.org/2000/svg">
      <circle class="progress-background" r="50" cx="50" cy="50" fill="transparent"/>
      <circle class="progress-bar" r="50" cx="50" cy="50" fill="transparent" :stroke-dasharray="dashArray"
              :stroke-dashoffset="dashOffset"/>
    </svg>
    <slot></slot>
  </div>
</template>

<script>
  export default {
    props: {
      radius: {
        type: Number,
        default: 100
      },
      percent: {
        type: Number,
        default: 0
      }
    },
    data() {
      return {
        dashArray: Math.PI * 100
      }
    },
    computed: {
      dashOffset() {
        return (1 - this.percent) * this.dashArray
      }
    }
  }
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
  @import "~common/stylus/variable"
  .progress-circle
    position: relative
    circle
      stroke-width: 8px
      transform-origin: center
      &.progress-background
        transform: scale(0.9)
        stroke: $color-theme-d
      &.progress-bar
        transform: scale(0.9) rotate(-90deg)
        stroke: $color-theme
</style>
```

# 播放器模式切换功能实现
1. 添加common/js/util.js
```javascript
function getRandomInt(min, max) {
  return Math.floor(Math.random() * (max - min + 1) + min)
}

export function shuffle(arr) {
  let _arr = arr.slice()
  for (let i = 0; i < _arr.length; i++) {
    let j = getRandomInt(0, i)
    let t = _arr[i]
    _arr[i] = _arr[j]
    _arr[j] = t
  }
  return _arr
}
```

2. 在play.vue中添加方法
```javascript
changeMode() {
  const mode = (this.mode + 1) % 3
  this.setPlayMode(mode)
  let list = null
  if (mode === playMode.random) {
    list = shuffle(this.sequenceList)
  } else {
    list = this.sequenceList
  }
  this.resetCurrentIndex(list)
  this.setPlaylist(list)
},
resetCurrentIndex(list) {
  let index = list.findIndex((item) => {
    return item.id === this.currentSong.id
  })
  this.setCurrentIndex(index)
},
```

3. 在player.vue中添加方法
```javascript
end() {
  if (this.mode === playMode.loop) {
    this.loop()
  } else {
    this.next()
  }
},
loop() {
  this.$refs.audio.currentTime = 0
  this.$refs.audio.play()
},
```

4. 在store/action中添加乱序播放方法
```javascript
export const randomPlay = function ({commit}, {list}) {
  commit(types.SET_PLAY_MODE, playMode.random)
  commit(types.SET_SEQUENCE_LIST, list)
  let randomList = shuffle(list)
  commit(types.SET_PLAYLIST, randomList)
  commit(types.SET_CURRENT_INDEX, 0)
  commit(types.SET_FULL_SCREEN, true)
  commit(types.SET_PLAYING_STATE, true)
}
```

5. 在music-list.vue中添加方法
```javascript
random() {
  this.randomPlay({
    list: this.songs
  })
},
```

6. 修改store/action
```javascript
function findIndex(list, song) {
  return list.findIndex((item) => {
    return item.id === song.id
  })
}

export const selectPlay = function({ commit, state }, { list, index }) {
  commit(types.SET_SEQUENCE_LIST, list)
  if (state.mode === playMode.random) {
    let randomList = shuffle(list)
    commit(types.SET_PLAYLIST, randomList)
    index = findIndex(randomList, list[index])
  } else {
    commit(types.SET_PLAYLIST, list)
  }
  commit(types.SET_CURRENT_INDEX, index)
  commit(types.SET_FULL_SCREEN, true)
  commit(types.SET_PLAYING_STATE, true)
}
```

##  播放器歌词数据抓取
1. 添加api/song.js
```javascript
import {commonParams} from './config'
import axios from 'axios'

export function getLyric(mid) {
  const url = '/api/lyric'

  const data = Object.assign({}, commonParams, {
    songmid: mid,
    platform: 'yqq',
    hostUin: 0,
    needNewCode: 0,
    categoryId: 10000000,
    pcachetime: +new Date(),
    format: 'json'
  })

  return axios.get(url, {
    params: data
  }).then((res) => {
    return Promise.resolve(res.data)
  })
}
```

2. build/dev-server.js添加方法
```javascript
apiRoutes.get('/lyric', function (req, res) {
  var url = 'https://c.y.qq.com/lyric/fcgi-bin/fcg_query_lyric_new.fcg'

  axios.get(url, {
    headers: {
      referer: 'https://c.y.qq.com/',
      host: 'c.y.qq.com'
    },
    params: req.query
  }).then((response) => {
    var ret = response.data
    if (typeof ret === 'string') {
      var reg = /^\w+\(({[^()]+})\)$/
      var matches = ret.match(reg)
      if (matches) {
        ret = JSON.parse(matches[1])
      }
    }
    res.json(ret)
  }).catch((e) => {
    console.log(e)
  })
})
```

3. song.js class 添加获取歌词方法
``` javascript
getLyric() {
  if (this.lyric) {
    return Promise.resolve(this.lyric)
  }

  return new Promise((resolve, reject) => {
    getLyric(this.mid).then((res) => {
      if (res.retcode === ERR_OK) {
        this.lyric = Base64.decode(res.lyric)
        resolve(this.lyric)
      } else {
        reject('no lyric')
      }
    })
  })
}
```

## 播放器歌词滚动列表实现
```javascript
handleLyric({lineNum, txt}) {
  this.currentLineNum = lineNum
  // 超过5行, 滚动到lineNum - 5, 5行之内, 滚动到顶部
  if (lineNum > 5) {
    let lineEl = this.$refs.lyricLine[lineNum - 5]
    this.$refs.lyricList.scrollToElement(lineEl, 1000)
  } else {
    this.$refs.lyricList.scrollTo(0, 0, 1000)
  }
  this.playingLyric = txt
},
```

## 播放器歌词左右滑动实现
1. created时添加变量touch
```javascript
created() {
  this.touch = {}
},
```

2. 添加滑动事件
```javascript
middleTouchStart(e) {
  this.touch.initiated = true
  // 用来判断是否是一次移动
  this.touch.moved = false
  const touch = e.touches[0]
  this.touch.startX = touch.pageX
  this.touch.startY = touch.pageY
},
middleTouchMove(e) {
  if (!this.touch.initiated) {
    return
  }
  const touch = e.touches[0]
  const deltaX = touch.pageX - this.touch.startX
  const deltaY = touch.pageY - this.touch.startY
  if (Math.abs(deltaY) > Math.abs(deltaX)) {
    return
  }
  if (!this.touch.moved) {
    this.touch.moved = true
  }
  const left = this.currentShow === 'cd' ? 0 : -window.innerWidth
  const offsetWidth = Math.min(0, Math.max(-window.innerWidth, left + deltaX))
  this.touch.percent = Math.abs(offsetWidth / window.innerWidth)
  this.$refs.lyricList.$el.style[transform] = `translate3d(${offsetWidth}px,0,0)`
  this.$refs.lyricList.$el.style[transitionDuration] = 0
  this.$refs.middleL.style.opacity = 1 - this.touch.percent
  this.$refs.middleL.style[transitionDuration] = 0
},
middleTouchEnd() {
  if (!this.touch.moved) {
    return
  }
  let offsetWidth
  let opacity
  if (this.currentShow === 'cd') {
    // 从右向左滑, 超过10%, 滚动屏幕
    if (this.touch.percent > 0.1) {
      offsetWidth = -window.innerWidth
      opacity = 0
      this.currentShow = 'lyric'
    } else {
      offsetWidth = 0
      opacity = 1
    }
  } else {
    if (this.touch.percent < 0.9) {
      offsetWidth = 0
      this.currentShow = 'cd'
      opacity = 1
    } else {
      offsetWidth = -window.innerWidth
      opacity = 0
    }
  }
  const time = 300
  this.$refs.lyricList.$el.style[transform] = `translate3d(${offsetWidth}px,0,0)`
  this.$refs.lyricList.$el.style[transitionDuration] = `${time}ms`
  this.$refs.middleL.style.opacity = opacity
  this.$refs.middleL.style[transitionDuration] = `${time}ms`
  this.touch.initiated = false
},
```

## 播放器歌词剩余功能实现

## 播放器底部播放器适配+mixin的应用
添加common/js/mixin.js
```javascript
import {mapGetters} from 'vuex'

export const playlistMixin = {
  computed: {
    ...mapGetters([
      'playlist'
    ])
  },
  mounted() {
    this.handlePlaylist(this.playlist)
  },
  activated() {
    this.handlePlaylist(this.playlist)
  },
  watch: {
    playlist(newVal) {
      this.handlePlaylist(newVal)
    }
  },
  methods: {
    handlePlaylist() {
      throw new Error('component must implement handlePlaylist method')
    }
  }
}
```

```javascript
// 引入mixin
import {playlistMixin} from 'common/js/mixin'

// 使用mixin
mixins: [playlistMixin],

// 添加handlePlaylist方法
handlePlaylist(playlist) {
const bottom = playlist.length > 0 ? '60px' : ''
  // this.$refs.list.$el 是 this.$refs.list的DOM元素
  this.$refs.list.$el.style.bottom = bottom
  this.$refs.list.refresh()
},
```

