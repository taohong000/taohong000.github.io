---
title: webpack使用场景总结
date: 2018-03-14 09:42:29
categories:
- 技术
tags:
- webpack
- 构建工具
---
# webpack使用总结

webpack 有四个**核心概念**
- 入口(entry)
- 输出(output)
- loader
- 插件(plugins)

下面主要讲解常见应用场景的配置

## 使用webpack方式
1. webpack命令
2. webpack配置
3. 第三方脚手架

## 编译 ES6
### 语法
  "babel-core": "^6.26.0"
  "babel-loader": "^7.1.4"
  "babel-preset-env": "^1.6.1"

### 函数和方法
- Babel Polyfill

全局垫片 为应用准备

``` bash
npm install babel-polyfill --save
```
``` javascript
import 'babel-polyfill'
```

- Babel Runtime Transform

局部垫片 为开发框架准备

``` bash
npm install babel-plugin-transform-runtime --save-dev
npm install babel-runtime --save
```
在 .babelrc中配置
``` json
{
  "presets": [
    ["@babel/preset-env", {
      "targets": {
        "borwsers": ["> 1%", "last 2 versions"]
      }
    }]
  ],
  "plugins" :["@babel/transform-runtime"]
}
```
## 编译 Typescript
typesript-loader

- 安装
``` bash
npm i typescipt ts-loader  --save-dev
// 或者
npm i typescipt awesome-typescript-loader  --save-dev
```

- 配置
```
tsconfig.json
webpack.config.js
```

## 提取公共代码
### 配置
1. webpack3

  [链接地址](https://doc.webpack-china.org/plugins/commons-chunk-plugin/)

2. webpack4

  webpack 4 移除 `CommonsChunkPlugin`，取而代之的是两个新的配置项（optimization.splitChunks 和 optimization.runtimeChunk）

  默认模式是经过千挑万选的，可以用于满足最佳web性能的策略。

  [没有了CommonsChunkPlugin，咱拿什么来分包](http://blog.csdn.net/songluyi/article/details/79419118)

  [精读《webpack4.0 升级指南》](https://github.com/dt-fe/weekly/blob/master/47.%E7%B2%BE%E8%AF%BB%E3%80%8Awebpack4.0%20%E5%8D%87%E7%BA%A7%E6%8C%87%E5%8D%97%E3%80%8B.md)


### 场景
- 单页应用
- 单页应用 + 第三方依赖
- 多页应用 + 第三方依赖 + webpack生成代码

## 代码分割 和 懒加载

### webpack methods （webpack内置方法）
require.ensure

require.include

### ES 2015 Loader spec （2015 loader 规范）

``` javascript
import()
```

## 处理CSS style-loader

### 引入
#### style-loader
1. style-loader
2. style-loader/url
3. style-loader/useable

Style-loader 可以使得我们把css 通过style 标签引入到我们的html 中去

Style-loader/url 可以让我们把css 通过 link 标签引入到我们的 html 中去

#### css-loader
options

| 配置选项      | 说明            |  
| :--------    | :--------       |
| alias        | 解析的别名       |
| importLoader | @import         |
| Minimize     | 是否压缩         |
| modules      | 启用css-modules |

如果设置了 root 查询参数，那么此查询参数将被添加到 URL 前面，然后再进行转译。
要禁用 css-loader 解析 url()，将选项设置为 false。


### CSS modules
1. :local
2. :global
3. compose
4. compose … from path

`localIdentName: '[path][name]__[local]--[hash:base64:5]'`


### 配置 less / sass
``` bash
npm install less-loader less  --save-dev
npm install sass-loader node-sass --save-dev
```

### 提取 CSS 代码
1. extract-loader
2. ExtractTextWebpackPlugin

``` bash
// webpack3
npm install extract-text-webpack-plugin --save-dev

// webpack4
npm install extract-text-webpack-plugin@next --save-dev
```

## PostCSS
转换css

### Autoprefixer

### CSS-nano

### CSS-next

## Tree Shaking
### JS Tree Shaking
### CSS Tree Shaking

## 文件处理
### 图片处理
1. css中引入的图片
2. 自动合成雪碧图
3. 压缩图片
4. base64 编码

file-loader(引入图片)
``` javascript
module: {
  rules: [
    {
      test: /\.(png|jpg|jpeg|gif)$/,
      use: [
          {
            loader: 'file-loader',
            options: {
              publishPath: '',
              outputPath: '',
              useRelativePath: true
            }
          }
        ]
    }
  ]
}
```

url-loader(base64 编码)
``` javascript
module.exports = {
  module: {
    rules: [
      {
        test: /\.(png|jpg|gif)$/,
        use: [
          {
            loader: 'url-loader',
            options: {
              limit: 5000,
              publishPath: '',
              outputPath: '',
              useRelativePath: true
            }
          }
        ]
      }
    ]
  }
}
```

img-loader(压缩图片)
[链接地址](https://www.npmjs.com/package/img-loader)

img-loader 是一个插件的集合，查看配置选项要到相应的插件中去看

``` javascript
{
  loader: 'url-loader',
  options: {
    pngquant: 80
  }
}
```

postcss-sprites(合成雪碧图)
``` javascript
{
  loader: 'postcss-loader',
  options: {
    ident: 'postcss',
    plugins: [
      require('postcss-sprites')({
        spritePath: 'dist/assets/imgs/sprites',
        retina: true
      })
    ]
  }
}
```

### 字体文件
``` javascript
{ test: /\.(eot|woff2?|ttf|svg)/
  user: 'url-loader',
  options: {
    limit: 5000,
    publishPath: '',
    outputPath: '',
    useRelativePath: true
  }
}
```

### 第三方js库
webpack.ProvidePlugin
``` javascript
plugins: [
  new webpack.ProvidePlugin({
    jquery$: 'jquery'
  })
]

// 本地文件
resolve: {
  alias: {
    jquery$: path.resolve(__dirname, 'src/libs/jquery.min.js')
  }
}
```

import-loader
``` javascript
{
  test: path.resolve(__dirname, 'src/app.js')
  user: 'imports-loader',
  options: {
    jquery$: 'jquery'
  }
}
```

window

## 生成 html
[链接](https://doc.webpack-china.org/plugins/html-webpack-plugin)
