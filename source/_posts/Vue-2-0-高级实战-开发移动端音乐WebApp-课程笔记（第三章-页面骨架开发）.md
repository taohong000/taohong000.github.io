---
title: Vue 2.0 高级实战-开发移动端音乐WebApp 课程笔记（第三章 页面骨架开发）
date: 2017-07-20 23:18:01
categories: 
- 技术
- 课程笔记
tags: 
- 慕课网
- vue
---
# Vue 2.0 高级实战-开发移动端音乐WebApp 课程笔记（第三章 页面骨架开发）

## 页面入口+header 组件的编写

### index.js 添加 viewport
```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <meta name="viewport"
          content="width=device-width, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0, user-scalable=no">
    <title>vue-music</title>
  </head>
  <body>
    <div id="app"></div>
    <!-- built files will be auto injected -->
  </body>
</html>
```

### package.json 添加依赖
```json
{
  "dependencies": {
	 "babel-runtime": "^6.0.0", // es语法转义
    "fastclick": "^1.0.6" // 解决 移动端点击300毫秒延迟的问题
  },
  "devDependencies": {
    "babel-polyfill": "^6.2.0" // babel补丁，可以使用es6的一些api
  },
}
```

###  /src/main.js 引入fastclick
``` javascript
import 'babel-polyfill'
import Vue from 'vue'
import App from './App'
import router from './router'

// 引入fastclick
import fastclick from 'fastclick'

import 'common/stylus/index.styl'

// 使用fastclick
fastclick.attach(document.body)

/* eslint-disable no-new */
new Vue({
  el: '#app',
  render: h => h(App)
})
```

### 创建m-header组件
```html
<template>
  <div class="m-header">
    <div class="icon"></div>
    <h1 class="text">Chicken Music</h1>
  </div>
</template>

<script type="text/ecmascript-6">
  export default {}
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
  @import "~common/stylus/variable"
  @import "~common/stylus/mixin"
  .m-header
    position: relative
    height: 44px
    text-align: center
    color: $color-theme
    font-size: 0
    .icon
      display: inline-block
      vertical-align: top
      margin-top: 6px
      width: 30px
      height: 32px
      margin-right: 9px
      bg-image('logo')
      background-size: 30px 32px
    .text
      display: inline-block
      vertical-align: top
      line-height: 44px
      font-size: $font-size-large
    .mine
      position: absolute
      top: 0
      right: 0
      .icon-mine
        display: block
        padding: 12px
        font-size: 20px
        color: $color-theme
</style>
```

### App.vue 引入Header组件

```html
<template>
  <div id="app" @touchmove.prevent>
    <m-header></m-header>
  </div>
</template>

<script type="text/ecmascript-6">
  import MHeader from 'components/m-header/m-header'
  export default {
    components: {
      MHeader
    }
  }
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
</style>
```

### webpack.base.conf.js 别名配置

```javascript
 resolve: {
    extensions: ['.js', '.vue', '.json'],
    alias: {
      '@': resolve('src'),
      'common': resolve('src/common'),
      'components': resolve('src/components')
    }
  },
```

##  路由配置+ tab 顶导组件开发

### router初始化

```javascript
import Vue from 'vue'
import Router from 'vue-router'
import Recommend from 'components/recommend/recommend'
import Singer from 'components/singer/singer'
import Rank from 'components/rank/rank'
import Search from 'components/search/search'

Vue.use(Router)

export default new Router({
  routes: [
	 // 根路径配置
    {
      path: '/',
      redirect: '/recommend'
    },
    {
      path: '/recommend',
      component: Recommend
    },
    {
      path: '/singer',
      component: Singer
    },
    {
      path: '/rank',
      component: Rank
    },
    {
      path: '/search',
      component: Search
    }
  ]
})
```

### main.js 引入router

```javascript
import 'babel-polyfill'
import Vue from 'vue'
import App from './App'
import router from './router'
import fastclick from 'fastclick'

import 'common/stylus/index.styl'

fastclick.attach(document.body)

/* eslint-disable no-new */
new Vue({
  el: '#app',
  router,
  render: h => h(App)
})
```

### App.vue 中使用router

```html
<template>
  <div id="app" @touchmove.prevent>
    <m-header></m-header>
    <router-view></router-view>
  </div>
</template>
```

### 创建tab.vue 组件

```html
<template>
  <div class="tab">
	// tag: 渲染的标签
    <router-link tag="div" class="tab-item" to="/recommend">
      <span class="tab-link">推荐</span>
    </router-link>
    <router-link tag="div" class="tab-item" to="/singer">
      <span class="tab-link">歌手</span>
    </router-link>
    <router-link tag="div" class="tab-item" to="/rank">
      <span class="tab-link">排行
      </span>
    </router-link>
    <router-link tag="div" class="tab-item" to="/search">
      <span class="tab-link">搜索</span>
    </router-link>
  </div>
</template>

<script type="text/ecmascript-6">
  export default {}
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
  @import "~common/stylus/variable"
  .tab
    display: flex
    height: 44px
    line-height: 44px
    font-size: $font-size-medium
    .tab-item
      flex: 1
      text-align: center
      .tab-link
        padding-bottom: 5px
        color: $color-text-l
      &.router-link-active
        .tab-link
          color: $color-theme
          border-bottom: 2px solid $color-theme
</style>
```

### App.vue 引入tab组件
```html
<template>
  <div id="app" @touchmove.prevent>
    <m-header></m-header>
    <tab></tab>
     <router-view></router-view>
  </div>
</template>

<script type="text/ecmascript-6">
  import MHeader from 'components/m-header/m-header'
  import Tab from 'components/tab/tab'

  export default {
    components: {
      MHeader,
      Tab
    }
  }
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
</style>
```