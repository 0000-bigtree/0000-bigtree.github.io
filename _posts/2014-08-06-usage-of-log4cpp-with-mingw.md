---
layout: post
title: "在 MinGW 环境下编译 log4cpp"
date: 2014-08-06 03:22:59 +0000
comments: true
sharing: false
categories: C/C++ MinGW log4cpp
---

从 [http://sourceforge.net/projects/log4cpp](http://sourceforge.net/projects/log4cpp) 下载源码，当前的最新版本的 1.1.1。
解压到某个文件夹，假定该文件夹为 `D:\workspace\log4cpp`。

执行 msys.bat，进入 MSYS，从MSYS 中进入 log4cpp 源文件所在目录：

    user@host ~
    $ cd /d/workspace/logcpp
    user@host /d/workspace/log4cpp
    $
    
执行 ./configure，开始配置：

    user@host /d/workspace/log4cpp
    $ ./configure

配置检查无误，会生成 Makefile 文件，开始编译：

    user@host /d/workspace/log4cpp
    $ make
    
结果编译失败，出现错误：

    ...
    ../include/log4cpp/config-MinGW32.h:20:17: error: 'long long long' is too long for GCC
     #define int64_t __int64
                     ^
    ../include/log4cpp/config-MinGW32.h:20:17: error: 'long long long' is too long for GCC
     #define int64_t __int64
                     ^
    ../include/log4cpp/config-MinGW32.h:20:17: error: declaration does not declare anything [-fpermissive]
    make[1]: *** [Appender.lo] Error 1
    ...
    
使用万能的 Google，找到了错误解决办法，文章在[这里](http://zhjxue.wordpress.com/2010/03/26/log4cpp-buildcompile-in-mingw-envrionment)，
简单来说就是注释掉 log4cpp 源码中 config-MinGW32.h 的第20行：“#define int64_t __int64”。

按前述重新执行 make 命令，虽然有一些警告，但终于编译通过了，执行 make install 将编译好的库和头文件拷贝到 MinGW 环境
(注意，确切来说，这个位置是 MSYS 环境的 `/local`，在使用 log4cpp 时要注意一点)：

    user@host /d/workspace/log4cpp
    $ make install   

- - -

修改历史

1.0.0，bigtree，2014年8月6日  
created  

1.0.0，bigtree，2014年8月6日  
release

1.0.0，bigtree，2015年5月26日  
迁移到 Jekyll，符合 GFM 的语法

