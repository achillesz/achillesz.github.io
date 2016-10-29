---
layout: post
title: "react native hello world"
keywords: react native
description: react native 安装
date: 2016-10-29 10:31:36
categories: [js]
tags: [react]
---

#### 安装部分

1. mac os.
2. xcode.
3. node nvm 
4. homebrew(mac下的安装工具)
5. [watchman](https://github.com/facebook/watchman#watchman) (`Watchman exists to watch files and record when they actually change. It can also trigger actions (such as rebuilding assets) when matching files change.`)
6. flow (`Flow, a static type checker for JavaScript, version 0.26.0`)

```text
    node -v
    nvm -v
    brew install watchman
    brew install flow
    watchman -v
    flow --version
```

```text
  npm install -g react-native-cli  
  react-native -v
```

```text
   git clone https://github.com/creationix/nvm
   cd nvm //新版没有该目录
   source nmv.sh
   nvm ls-remote
   nvm ls  //当前本机安装的所有node版本
```

react 希望将功能分解化,让开发变得像搭积木一样。

#### 开始一个简单项目

```text
    react-native init Hello // 进入工作目录,新建一个项目
```

```text
To run your app on iOS:
   cd /Users/user/fe/Hello
   react-native run-ios
   - or -
   Open /Users/user/fe/Hello/ios/Hello.xcodeproj in Xcode
   Hit the Run button
To run your app on Android:
   Have an Android emulator running (quickest way to get started), or a device connected
   cd /Users/user/fe/Hello
   react-native run-android
```


```text
    react-native run-ios
```

执行情况

 ![执行情况](/assets/img/hellowrold.png)
 
 报错处理
 
 ![报错处理](/assets/img/run-error.png)
 
 进入项目目录,找到react-native, `npm start`
 
 选择 `reload js`
 
 修改 `index.ios.js` 
 
 
 成功:
 
 ![成功](/assets/img/hello-success.png)
 
 
  
  

 
 
 
 

  