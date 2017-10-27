---
title: Vue 2.0 高级实战-开发移动端音乐WebApp 课程笔记（第12章 用户中心页面）
date: 2017-08-16 14:29:23
categories: 
- 技术
- 课程笔记
tags: 
- 慕课网
- vue
---

# Vue 2.0 高级实战-开发移动端音乐WebApp 课程笔记（第12章 用户中心页面）


## 用户中心页面布局和功能介绍
组件基本dom结构
```html
<template>
  <transition name="slide">
    <div class="user-center">
      <!-- 返回按钮 start -->
      <div class="back">
        <i class="icon-back"></i>
      </div>
      <!-- 返回按钮 end -->
      <!-- switch按钮 start -->
      <div class="switches-wrapper">
      </div>
      <!-- switch按钮 end -->
      <!-- 随机播放按钮 start -->
      <div ref="playBtn" class="play-btn" @click="random">
        <i class="icon-play"></i>
        <span class="text">随机播放全部</span>
      </div>
      <!-- 随机播放按钮 end -->
      <!-- 列表 start -->
      <div class="list-wrapper" ref="listWrapper">
      </div>
      <!-- 列表 end -->
    </div>
  </transition>
</template>
```

## 用户中心页面收藏列表的Vuex数据设计和实现
```javascript
// state
favoriteList: loadFavorite()

// mutation-type
export const SET_FAVORITE_LIST = 'SET_FAVORITE_LIST'

// mutation
[types.SET_FAVORITE_HISTORY](state, list) {
  state.favoriteList = list
}

// getters
export const favoriteList = state => state.favoriteList

// actions
export const saveFavoriteList = function({ commit }, song) {
  commit(types.SET_FAVORITE_LIST, saveFavorite(song))
}

export const deleteFavoriteList = function({ commit }, song) {
  commit(types.SET_FAVORITE_LIST, deleteFavorite(song))
}
```

## 用户中心页面收藏歌曲功能实现
```html
<template>
  <transition name="slide">
    <div class="user-center">
      <!-- 返回按钮 start -->
      <div class="back">
        <i class="icon-back"></i>
      </div>
      <!-- 返回按钮 end -->
      <!-- switch按钮 start -->
      <div class="switches-wrapper">
        <switches @switch="swtichItem" :switches="switches" :currentIndex="currentIndex"></switches>
      </div>
      <!-- switch按钮 end -->
      <!-- 随机播放按钮 start -->
      <div ref="playBtn" class="play-btn">
        <i class="icon-play"></i>
        <span class="text">随机播放全部</span>
      </div>
      <!-- 随机播放按钮 end -->
      <!-- 列表 start -->
      <div class="list-wrapper" ref="listWrapper">
        <scroll ref="favoriteList" class="list-scroll" v-if="currentIndex===0" :data="favoriteList">
          <div class="list-inner">
            <song-list :songs="favoriteList" @select="selectSong"></song-list>
          </div>
        </scroll>
        <scroll ref="playList" class="list-scroll" v-if="currentIndex===1" :data="playHistory">
          <div class="list-inner">
            <song-list :songs="playHistory" @select="selectSong"></song-list>
          </div>
        </scroll>
      </div>
      <!-- 列表 end -->
    </div>
  </transition>
</template>

<script>
  import Switches from 'base/switches/switches'
  import Scroll from 'base/scroll/scroll'
  import SongList from 'base/song-list/song-list'
  import Song from 'common/js/song'
  import {mapGetters, mapActions} from 'vuex'

  export default {
    data() {
      return {
        currentIndex: 0,
        switches: [
          {name: '我喜欢的'},
          {name: '最近听的'}
        ]
      }
    },
    computed: {
      ...mapGetters([
        'favoriteList',
        'playHistory'
      ])
    },
    methods: {
      swtichItem(index) {
        this.currentIndex = index
      },
      selectSong(song) {
        this.insertSong(new Song(song))
      },
      ...mapActions([
        'insertSong'
      ])
    },
    components: {
      Switches,
      Scroll,
      SongList
    }
  }
</script>
```

## 用户中心页面剩余功能实现
```html 
<template>
  <transition name="slide">
    <div class="user-center">
      <!-- 返回按钮 start -->
      <div class="back" @click="back">
        <i class="icon-back"></i>
      </div>
      <!-- 返回按钮 end -->
      <!-- switch按钮 start -->
      <div class="switches-wrapper">
        <switches @switch="swtichItem" :switches="switches" :currentIndex="currentIndex"></switches>
      </div>
      <!-- switch按钮 end -->
      <!-- 随机播放按钮 start -->
      <div ref="playBtn" class="play-btn" @click="random">
        <i class="icon-play"></i>
        <span class="text">随机播放全部</span>
      </div>
      <!-- 随机播放按钮 end -->
      <!-- 列表 start -->
      <div class="list-wrapper" ref="listWrapper">
        <scroll ref="favoriteList" class="list-scroll" v-if="currentIndex===0" :data="favoriteList">
          <div class="list-inner">
            <song-list :songs="favoriteList" @select="selectSong"></song-list>
          </div>
        </scroll>
        <scroll ref="playlist" class="list-scroll" v-if="currentIndex===1" :data="playHistory">
          <div class="list-inner">
            <song-list :songs="playHistory" @select="selectSong"></song-list>
          </div>
        </scroll>
      </div>
      <!-- 列表 end -->
      <div class="no-result-wrapper" v-show="noResult">
        <no-result :title="noResultDesc"></no-result>
      </div>
    </div>
  </transition>
</template>

<script>
  import Switches from 'base/switches/switches'
  import Scroll from 'base/scroll/scroll'
  import SongList from 'base/song-list/song-list'
  import NoResult from 'base/no-result/no-result'
  import Song from 'common/js/song'
  import {mapGetters, mapActions} from 'vuex'
  import {playlistMixin} from 'common/js/mixin'

  export default {
    mixins: [playlistMixin],
    data() {
      return {
        currentIndex: 0,
        switches: [
          {name: '我喜欢的'},
          {name: '最近听的'}
        ]
      }
    },
    computed: {
      noResult() {
        if (this.currentIndex === 0) {
          return !this.favoriteList.length
        } else {
          return !this.playlist.length
        }
      },
      noResultDesc() {
        if (this.currentIndex === 0) {
          return '暂无收藏歌曲'
        } else {
          return '你还没有听过歌曲'
        }
      },
      ...mapGetters([
        'favoriteList',
        'playHistory'
      ])
    },
    methods: {
      handlePlaylist(playlist) {
        const bottom = playlist.length > 0 ? '60px' : ''
        this.$refs.listWrapper.style.bottom = bottom
        this.$refs.$favoriteList && this.$refs.$favoriteList.refresh()
        this.$refs.$playlist && this.$refs.$playlist.refresh()
      },
      swtichItem(index) {
        this.currentIndex = index
      },
      selectSong(song) {
        this.insertSong(new Song(song))
      },
      back() {
        this.$router.back()
      },
      random() {
        let list = this.currentIndex === 0 ? this.favoriteList : this.playHistory
        if (list.length === 0) {
          return
        }
        // 为什么用song的实例, song的实例上不仅有属性, 还有方法
        list = list.map(song => new Song(song))
        this.randomPlay({list})
      },
      ...mapActions([
        'insertSong',
        'randomPlay'
      ])
    },
    components: {
      Switches,
      Scroll,
      SongList,
      NoResult
    }
  }
</script>
```