---
title: react native 导航器
categories:
  - 技术
date: 2020-08-21 14:52:03
tags:
- react native
---

> https://reactnavigation.org/

## 安装导航器
安装核心包
```
yarn add @react-navigation/native
```

安装其它依赖
```
yarn add react-native-reanimated react-native-gesture-handler react-native-screens react-native-safe-area-context @react-native-community/masked-view
```

处理原生内容
ios
```
npx pod-install ios
```

android

处理手势库
> https://docs.swmansion.com/react-native-gesture-handler/docs/

`MainActivity.java`
``` java
package com.swmansion.gesturehandler.react.example;

import com.facebook.react.ReactActivity;
+ import com.facebook.react.ReactActivityDelegate;
+ import com.facebook.react.ReactRootView;
+ import com.swmansion.gesturehandler.react.RNGestureHandlerEnabledRootView;
public class MainActivity extends ReactActivity {

  @Override
  protected String getMainComponentName() {
    return "Example";
  }
+  @Override
+  protected ReactActivityDelegate createReactActivityDelegate() {
+    return new ReactActivityDelegate(this, getMainComponentName()) {
+      @Override
+      protected ReactRootView createRootView() {
+       return new RNGestureHandlerEnabledRootView(MainActivity.this);
+      }
+    };
+  }
}
```

导入 index.js 或 App.js
```
import 'react-native-gesture-handler';
```

## 堆栈式导航器

> https://reactnavigation.org/docs/hello-react-navigation

安装
```
yarn add @react-navigation/stack
```


使用
``` javascript
// In App.js in a new project

import * as React from 'react';
import { View, Text } from 'react-native';
import { NavigationContainer } from '@react-navigation/native';
import { createStackNavigator } from '@react-navigation/stack';

function HomeScreen() {
  return (
    <View style={{ flex: 1, alignItems: 'center', justifyContent: 'center' }}>
      <Text>Home Screen</Text>
    </View>
  );
}

const Stack = createStackNavigator();

function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator>
        <Stack.Screen name="Home" component={HomeScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}

export default App;
```
## 底部导航器

> https://reactnavigation.org/docs/bottom-tab-navigator

安装
```
yarn add @react-navigation/bottom-tabs
```

使用
``` javascript
import { createBottomTabNavigator } from '@react-navigation/bottom-tabs';

const Tab = createBottomTabNavigator();

function MyTabs() {
  return (
    <Tab.Navigator>
      <Tab.Screen name="Home" component={HomeScreen} />
      <Tab.Screen name="Settings" component={SettingsScreen} />
    </Tab.Navigator>
  );
}
```
