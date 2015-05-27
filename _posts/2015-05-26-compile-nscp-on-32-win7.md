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

## 编译时需要的软件说明

按照官网的[说明](http://docs.nsclient.org/manual/build.html#windows)，需要先安装以下软件：

* CMake 2.6

这个是一个跨平台的构建系统。用来构建 NSClient++ 的。

* Python 2.7

作用一是运行一些 Python 写的构建脚本；二是提供库和头文件，来支持嵌入 Python 解释器。

* Visual Studio

* WiX 3.5

[WiX](http://wixtoolset.org)，用来创建 Windows 安装程序的工具。

* Nasm 2.10 (optinal)

[Nasm](http://www.nasm.us)，这个是一个 80x86、x86-64 汇编器，不知道在哪里用了。

* Perl 5.12 (required by openssl)

* Msysgit (latest version)

[Msysgit](http://msysgit.github.io)，用来从 Github 上下载源代码，编译时不需要。


## 安装所需要的软件

### 安装 CMake

    cinst cmake
    
我用了 [Chocolatey](https://chocolatey.org) 来安装 CMake，以简化安装过程。它安装的 CMake 版本是 3.3.2。

### 安装 Python 2

    cinst python2
    
当前 Python 的最新版本是 2.7.10

### 安装 Visual Studio

    cinst visualstudioexpress2013windows
    
Visual Studio Express 版可以免费使用，版本号是 2013，已经足够使用。

### 安装 WiX

    cinst wixtoolset
    
## 验证安装

    cmake --version
    python -V
    perl -v
    cl /?

## 获取并构建依赖的库

    md win32-build-folder 
    cd win32-build-folder 
    git clone --recursive https://github.com/mickem/nscp.git
    python nscp\build\python\fetchdeps.py --target win32 --cmake-config dist
    
执行 fetchdeps.py 脚本需要很长时间，它会去下载工程的依赖库的源码。有些依赖库很庞大。
比如，它的一个依赖 boost，其源代码包 boost_1_56_0.zip 的大小是 161 M。
