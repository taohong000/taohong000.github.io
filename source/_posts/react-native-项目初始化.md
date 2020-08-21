---
title: react native 项目初始化
categories:
  - 技术
date: 2020-08-21 14:26:46
tags:
- react natvie
---

## 初始化项目
``` bash
npx react-native init projectname --template react-native-template-typescript

# 启动项目 
yarn ios
yarn android
```

## 多环境

> https://js.coach/package/react-native-config

``` bash
yarn add react-native-config
```

根目录下新建 `.env`
``` 
API_URL=https://myapi.com
GOOGLE_MAPS_API_KEY=abcdefgh
```

使用
``` javascript
import Config from "react-native-config";

Config.API_URL; // 'https://myapi.com'
Config.GOOGLE_MAPS_API_KEY; // 'abcdefgh'
```

安装

ios
```
(cd ios; pod install)
```

android
android/app/build.gradle
```
// 2nd line, add a new apply:
apply from: project(':react-native-config').projectDir.getPath() + "/dotenv.gradle"
```

## 绝对路径

> https://www.npmjs.com/package/babel-plugin-module-resolver

```
yarn add babel-plugin-module-resolver
```

配置 `.babelrc` 或 `babel.config.js`
``` javascript
module.exports = {
  presets: ['module:metro-react-native-babel-preset'],
  plugins: [
    [
      'module-resolver',
      {
        root: ['./src'],
        alias: {
          '@': './src',
        },
      },
    ],
  ],
};

```

配置 `tsconfig.json`

``` json
{
  "baseUrl": "./src",                       
  "paths": {
    "@/*": ["*"],
  },
}
 
```


