---
layout: post
title: "React Native for Android 开发环境搭建"
date: 2015-11-01 23:59:59 +0800
comments: true
sharing: false
categories: react javascript android osx react-native
---

[React Native](https://github.com/facebook/react-native) 0.11.0 就已经开始支持 Android 了，但 Facebook 开始只支持 OSX 平台，0.13.0-rc 之后，终于支持 Windows 了。

本文使用的 React Native 版本为 0.13.0

# OS X

    brew install android-sdk

编辑 .zshrc，添加

    export ANDROID_HOME=/usr/local/opt/android-sdk

安装完成之后，输入命令，

    android

出现 Android SDK Manager，使用它来安装必要的 SDK 组件，注意要安装 Android SDK Build-tools 23.0.1，后面由 react-native　创建的工程中，Android 使用的是这个版本，另外安装时需要代理。

安装完成后，进入到创建的 React Native 工程目录中，

    react-native run-android

会启动 JS Server，并运行 gradle 命令(自动安装 Gradle 2.4)。
