---
layout: post
title: "配置 Emacs"
date: 2015-06-26 15:31:00 +0800
comments: true
sharing: false
categories: emacs
---

# Emacs

9 年前就开始用，不算重度用户，只是偶尔使用，有些知识掌握不牢固，导致有些东西需要重复学习。
写篇文档记录一下，节省时间。

# Windows 版本及配置

当前的最新版本是 [24.5](https://ftp.gnu.org/gnu/emacs/windows/emacs-24.5-bin-i686-mingw32.zip)。

解压后，目录结果如下所示，这样的结构便于版本升级，在各个操作系统下使用统一的配置文件，

    D:\workspace\coder
      emacs\
        24.5-i686-mingw32\
        home\
        personal\
          preload\
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

使用 [Emacs Prelude](https://github.com/bbatsov/prelude)，

    cd D:\workspace\coder\emacs\home
    git clone git://github.com/bbatsov/prelude.git .emacs.d

在 personal 中创建基本的配置文件，作为 Emacs Prelude 的补充，

basic.el 的内容为，

    ;; user name
    (setq user-full-name "WDS")

    ;; set default utf-8 encoding
    (prefer-coding-system 'utf-8)

    ;; set font size
    (set-face-attribute 'default nil :height 140)
    ;; 标题栏显示当前的 buffer 名
    (setq frame-title-format "emacs@%b")
    ;; 在 minibuffer 上面显示时间
    (setq display-time-format "%a %b %d %H:%M:%S")
    (display-time-mode t)

    ;; 显示行号设置
    (require 'linum)
    (setq linum-format "%3d ")
    ;; 对所有文件生效
    (add-hook 'find-file-hooks (lambda () (linum-mode 1)))

将 `D:\workspace\coder\emacs\personal` 链接为 `D:\workspace\coder\emacs\home\.emacs.d\personal`

    junction.exe personal d:\workspace\coder\emacs\personal\

运行 emacs，使 Emacs Prelude 自动安装所需要要的项目(从 elpa)，
打开 groovy 文件，自动安装 groovy 支持，
打开 md 文件，自动安装 Markdown 支持，
打开 yaml 或 yml 文件，自动安装 YAML 支持，
安装 adoc-mode(M-x package-install adoc-mode)，
安装 color-theme，color-theme-solarized，monokai-theme。

# OS X 版本及配置

OS X 下有 Homebrew，比较方便，

    brew tap caskroom/cask/brew-cask && brew install brew-cask
    # brew untap caskroom/cask/brew-cask
    brew cask install emacs

参考　Windows 的配置，把 Emacs Prelude 克隆到 ~/.emacs.d，并安装相关的功能。

注意，在终端下运行 emacs，需要以下命令，

    export TERM=xterm-256color &&  ~/Applications/Emacs.app/Contents/MacOS/Emacs -nw "$@"

# Linux 版本及配置(ubuntu 14.04)

# Markdown Mode

有一篇[介绍文章](http://jblevins.org/projects/markdown-mode/)

# Rails

[configuring-emacs-for-rails)(http://lorefnon.me/2014/02/02/configuring-emacs-for-rails.html)
