---
layout: post
title: "在emacs Org mode环境中学习Ruby"
date: 2016-01-09 23:59:59 +0800
comments: true
sharing: false
categories: emacs "Org mode" ruby
---

# 什么是Org mode

按照官网[http://orgmode.org/](http://orgmode.org/)的说法，

> Org mode is for keeping notes, maintaining TODO lists, planning projects, and authoring documents with a fast and effective plain-text system.

简单来说，Org mode就是用来记笔记、维护待办列表、做计划以及写作文档的快速高效的纯文本系统。在一个org文档中，所有的信息都是以纯文本来存储的，具有良好的可读性及跨平台性。org文档的设计实际上是独立于emacs的，因为它是纯文本，只要遵从了它的一些简单语法规则，可以用任何文本编辑器来修改。

尽管org文档的设计可以与emacs无关，但其具体实现和运用来说，脱离了emacs这个环境，它的实际使用的效能可能会降低90%，emacs为org格式的文档提供了编辑、展现、发布、转换等相关的功能，使得两者相得益彰。

基于literate programming 的概念，org 提供了对源代码的相应操作支持。可以在org文档中支持多种编程语言的编辑、运行、代码块调用执行、运行结果导出等。它支持的语言为[http://orgmode.org/manual/Languages.html#Languages](http://orgmode.org/manual/Languages.html#Languages)。

本文写作时Org mode的版本为8.3.2。
