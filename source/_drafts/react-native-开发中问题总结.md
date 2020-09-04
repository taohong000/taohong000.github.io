---
title: react native 开发中问题总结
tags:
- react native
---

## @ant-design/react-native 缺少图标
文档没有更新

```
react-native link @ant-design/react-native
```

## 添加webView 打包 报错
``` bash
Task :app:mergeDexDebug FAILED
```

解决方案

[链接](https://github.com/react-native-community/react-native-webview/issues/1344)

At Path android/app/build.gradle
```
defaultConfig {
multiDexEnabled true //Add this line
}
```


## react native 和 webView 通信
[链接](https://github.com/react-native-community/react-native-webview/blob/master/docs/Guide.md#communicating-between-js-and-native)

1. React Native -> Web: The `injectedJavaScript` prop
2. React Native -> Web: The `injectJavaScript` method
3. Web -> React Native: The `postMessage` method and `onMessage` prop