---
layout: post
title:  "React Native 初步"
date:   2015-05-15 00:00:00
categories: React Native
---

== 介绍

== 第一个项目

开发环境是 OS X。

. 安装 node、 https://facebook.github.io/watchman[watchman]、 http://flowtype.org[flow]

    brew install node watchman flow
    
watchman 是开发时监测代码文件变化的；flow 是用来对 JavaScript 代码做静态类型检查的。

[start=2]
. 安装 react-native-cli。

    npm install -g react-native-cli    
    
.Node.js 说明
****
react-native-cli 是一个 npm module，react native 是利用 JS 来写移动应用的，
当然使用 node.js 的工具链，JavaScript 真是有大统一的趋势(或者说以前传统的 Web 技术栈不再局限于浏览器)。
JavaScript 的应用场合， 在浏览器上，从最初的简单 Web 特效，到复杂的库，如通用库 JQuery，界面框架 Dojo 等，
相应的是 Ajax 应用兴起，最近这几年，AngularJS、React 等类似框架不断涌现。

在服务端，Node.js 生态系统非常繁荣，相当多的 npm module，涵盖了开发所需要的方方面面，例如，有非常出名的 express Web 框架。

在桌面上，利用 Google Chromium 和 Node.js，发展出 NW.js 和 Github Electron，利用浏览器内核，以开发 Web 应用的方式
来开发桌面应用。

在移动设备上，有两种方式，一种方式跟桌面上的 Github Electron 类似，在移动 app 中，利用内嵌的 Google Chrome 来展示界面，
然后封装了移动平台的相关功能为 JS 库，供 JavaScript 代码调用。这类 app 称之为 混合应用(hybrid app)，
比较典型的有 http://ionicframework.com[Ionic]，目前支持 iOS 和 Android。
另一种方式就是 react native，使用了 Web 相关的技术栈，但 app 内部并没有内嵌浏览器，而是通过框架将 JS 的界面代码
转换为本地代码的界面，原理上来说，这种方式性能更高，灵活性也更强，但框架实现起来，相对复杂一些，基于这种方式来开发 app，
也要比 hybrid app 复杂一点。
****    
 
[start=3]
. 利用工具创建 XCode 工程

    react-native init AwesomeProject  

. 运行

上面的命令会创建一个目录 AwesomeProject，进入之后，双打开 AwesomeProject.xcodeproj，XCode 打开该工程后，
直接运行，模拟器中即可看到效果。

image::iphone.png[align="center"]

工程的目录结构很简单，

image::dir-structure.png[align="center"]

其中，package.json 是符合 npm 规范的一个模块工程文件，node_modules 是放工程依赖的 npm modules 的地方。
从这里可以看出，实际上这个目录也是一个标准 npm 模块的工程目录。

其他目录是 XCode 工程相关的目录，除非你需要在 react native 中集成一些 Objective-C 写的代码，
简单情况下，基本上不需要关注它们。

程序代码主要在 index.ios.js，只要了解一点 JavaScript、HTML 和 CSS 的知识，代码不言自明：

[source,javascript,linenums]
----
'use strict';

var React = require('react-native');
var {
  AppRegistry,
  StyleSheet,
  Text,
  View,
} = React;

var AwesomeProject = React.createClass({
  render: function() {
    return (
      <View style={styles.container}>
        <Text style={styles.welcome}>
          Welcome to React Native!
        </Text>
        <Text style={styles.instructions}>
          To get started, edit index.ios.js
        </Text>
        <Text style={styles.instructions}>
          Press Cmd+R to reload,{'\n'}
          Cmd+D or shake for dev menu
        </Text>
      </View>
    );
  }
});

var styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    backgroundColor: '#F5FCFF',
  },
  welcome: {
    fontSize: 20,
    textAlign: 'center',
    margin: 10,
  },
  instructions: {
    textAlign: 'center',
    color: '#333333',
    marginBottom: 5,
  },
});

AppRegistry.registerComponent('AwesomeProject', () => AwesomeProject);
----

[start=5]
. 修改无需重新编译运行

这个是比较像 Web 开发，并且大大提高效率的一点。略微修改一下代码：

[source,javascript,linenums]
----
...
      <View style={styles.container}>
        <Text style={styles.welcome}>
          Welcome to React Native! Copied Code by bigtree!
...
----

在模拟器上，按一下 Cmd+R，新的界面立即变化了！的确比较方便。

image::reload.png[align="center"]

