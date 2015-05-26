---
layout: post
title: "log4cplus 使用指南"
date: 2014-08-06 03:54:59 +0000
comments: true
sharing: false
categories: C/C++ MinGW log4cplus
---
# 介绍

## 在 MinGW 环境下编译 log4cplus

从 [http://sourceforge.net/projects/log4cplus](http://sourceforge.net/projects/log4cplus) 下载源码，
当前的最新版本的 1.1.3-rc2。解压到某个文件夹，假定该文件夹为 `D:\workspace\log4cplus`。

执行 msys.bat，进入 MSYS，从 MSYS 中进入 log4cplus 源文件所在目录：

    user@host ~
    $ cd /d/workspace/log4cplus
    $ ./configure
    $ make
    $ make install   
    
在使用时要注意，`make install` 会将 log4cplus 的头文件和库文件拷贝到 MSYS 环境的 `/local/include` 和 `/local/lib` 。

# 使用实例

- - -

# 修改历史

1.0.0，bigtree，2014年8月6日  
created  

1.0.0，bigtree，2014年8月6日  
release

1.0.0，bigtree，2015年5月26日  
迁移到 Jekyll，符合 GFM 的语法

