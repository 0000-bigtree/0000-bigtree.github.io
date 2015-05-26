---
layout: post
title: "在 32 位 Windows 7 下编译 NSClient++"
date: 2015-05-26 19:08:59 +0800
comments: true
sharing: false
categories: NSClient++ C++
---
# NSClient++ 简介

[NSClient++](http://www.nsclient.org/) 是一个监控客户端，由 C++ 编写，支持对 Linux 和 Windows
系统进行监控。另外，它还支持多种技术来扩展其功能，包括外部脚本、Lua 脚本、Python 脚本、.NET 模块和本地模块。
当前的最新稳定版是 0.4.3。它的源代码位于 Github：[https://github.com/mickem/nscp](https://github.com/mickem/nscp)。

选择 NSClient++ 进行学习的原因是：

* 它是基本上开源的(按官网的说法，它的核心不开源)。

* 在系统监控代理比较成熟，功能较为丰富，有一定的实际使用量。

* 涉及比较多的技术，包括内嵌 Lua、Python，.NET、模块化、Windows 安装程序、跨平台建脚本(使用 CMake)等。

* 代码规模适中，专注于监控方面，没有涉及太多的非核心业务和技术。

# 编译步骤

## 编译时需要的软件

按照官网的[说明](http://docs.nsclient.org/manual/build.html#windows)，需要先安装以下软件：

* CMake 2.6

这个是一个跨平台的构建系统。用来构建 NSClient++ 的。

* Python 2.7

这个估计是提供库和头文件，来支持嵌入 Python 解释器的。

* Visual Studio

* WiX 3.5

[WiX](http://wixtoolset.org)，用来创建 Windows 安装程序的工具。

* Nasm 2.10 (optinal)

[Nasm](http://www.nasm.us)，这个是一个 80x86、x86-64 汇编器，不知道在哪里用了。

* Perl 5.12 (required by openssl)

* Msysgit (latest version)

[Msysgit](http://msysgit.github.io)，用来从 Github 上下载源代码，编译时不需要。


## 具体过程

### 安装 CMake

    cinst cmake
    
我用了 [Chocolatey](https://chocolatey.org) 来安装 CMake，以简化安装过程。它安装的 CMake 版本是 3.3.2。




