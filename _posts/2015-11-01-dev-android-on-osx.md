---
layout: post
title: "React Native for Android 开发环境搭建"
date: 2015-11-01 23:59:59 +0800
comments: true
sharing: false
categories: react javaScript android osx react-native
---

[React Native](https://github.com/facebook/react-native) 0.11.0 就已经开始支持 Android 了，但 Facebook 开始只支持在 OSX 平台来开发，0.13.0-rc 之后，终于支持 Windows 了。

本文使用的 React Native 版本为 0.13.0，当前最新稳定版本。

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

# Windows

## 安装 Android SDK

由于 Android 官网被墙，去 [http://www.androiddevtools.cn](http://www.androiddevtools.cn) 下载最新的 SDK Tools。当前的最新版本是 24.3.4。

安装好 Android SDK Manager 后，打开 Tools->Manage Add-on Sites...，在 User Defined Sites 中，添加一个站点，http://android-mirror.bugly.qq.com:8080/android/repository/addon.xml。

打开 Android SDK Manager 的菜单，Tools->Options...，配置代理服务器为 android-mirror.bugly.qq.com，端口为 8080，勾选 “Force https://... sources to be fetched using http://...”。

选择所需要的组件安装，整个安装需要比较长的时间，如果你的网络速度不是很好的话，更需要耐心一点，下载安装完成后，添加环境变量 ANDROID_HOME，值为 C:\Program Files (x86)\Android\android-sdk。

## 安装 NDK(可选)

## 安装 Genymotion

[Genymotion](https://www.genymotion.com/#!/) 是一个虚拟设备模拟器，对于个人使用是免费的。比 Google 的模拟器要方便快捷。

## 安装 Node 及 npm

略，使用 node 4.2.1 及 npm 3.3.10

## 安装 react-native-cli

    npm install -g react-native-cli

## 创建工程

    react-native init helloReactNative

## 运行示例

### 使用真实设备运行

先把手机通过 USB 线连接好，打开 USB 调试，再执行如下命令，

    cd helloReactNative
    react-native run-android
    react-native start

会自动安装 Gradle 2.4，并自动下载依赖的 jar 包，编译成功后，自动把应用安装到手机上。

安装成功后，在手机上会启动应用，但会是一片空白，什么都没有显示，解决办法，根据 [这个链接](http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2015/0916/3467.html) 解决问题。

### 使用 Genymotion 模拟器

登录 Genymotion 并下载一个模拟器镜像，启动镜像。

    react-native run-android
    react-native start

安装好的应用会自动在模拟器上运行。
