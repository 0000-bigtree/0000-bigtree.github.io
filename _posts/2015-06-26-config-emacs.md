---
layout: post
title: "配置 Emacs"
date: 2015-06-26 15:31:00 +0800
comments: true
sharing: false
categories: emacs
---

# Emacs

9 年前就开始用，但只是偶尔使用，不算重度用户，有些知识掌握不牢固，导致有些东西需要重复学习。
写篇文档记录一下，节省时间。

# Windows 版本及配置

当前的最新版本是 [24.5](https://ftp.gnu.org/gnu/emacs/windows/emacs-24.5-bin-i686-mingw32.zip)。

解压后，目录结果如下所示，这样的结构便于版本升级，在各个操作系统下使用统一的配置文件，

    D:\workspace\coder
      emacs
        24.5-i686-mingw32
        home
        emacs.bat
        .gitignore
        
        
在 emacs 目录下新建 emacs.bat 文件，内容如下，

    @SETLOCAL
    @CD /D D:\workspace\coder\emacs\home
    SET HOME=%CD%
    @START %HOME%\..\24.5-i686-mingw32\bin\runemacs.exe --no-splash -mm %*
    @ENDLOCAL

emacs 会读取 HOME 系统变量，作为自己的主目录。启动 emacs 时，使用 runemacs.exe，
不会在 emacs 窗口后面出现控制台窗口。--no-splash 参数指定无启动闪屏，--mm 参数
指定窗口最大化。

编辑 .gitignore，过滤不需要放入 VCS 的文件和目录，

    24.5-i686-mingw32
    home/.backups
    home/.emacs.d
    home/.session



