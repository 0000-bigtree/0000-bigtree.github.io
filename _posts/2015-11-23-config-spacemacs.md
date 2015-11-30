---
layout: post
title: "配置 spacemacs"
date: 2015-11-23 18:00:00 +0800
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


# Source Code Pro 字体

[Source Code Pro](https://github.com/adobe-fonts/source-code-pro) 是 Adobe 开源的等宽字体，大家都说好，但我觉得在Windows上效果不怎么样，在OSX上，看起来的确是不错的。

Github上的README已经有很详细的安装方法，按照文档所述的步骤，下载对应操作系统的安装文件安装即可。

# the Platinum Searcher in Emacs

[pt.el](https://github.com/bling/pt.el)，主要用来做代码搜索的。

从这里取对应操作系统的版本来安装，[The Platinum Searcher](https://github.com/monochromegane/the_platinum_searcher)，pt有 Windows、Linux、OSX上现成的版本，相对于 ag、grep，在Windows上不需要安装MinGW或Cygiwn，比较方便。

# Java

## jdee

[JDEE](https://github.com/jdee-emacs/jdee)，以前比较好的一套插件，但很长时间没有更新，把代码库迁移到Github后，现在处于reborn状态，但目前进展比较缓慢，按照它的RoadMap，是要支持Maven、Gradle的，保留期待。

## emacs-eclim

[eclim 介绍](http://www.emacswiki.org/emacs/EmacsEclim)

[eclim github](https://github.com/senny/emacs-eclim)

### eclipse

下载最新的 4.5.1版本，地址为[http://www.eclipse.org/downloads/](http://www.eclipse.org/downloads/)，注意根据你操作系统的JDK 版本选择对应的64位或32位Eclipse版本。

安装必要的插件，ADT(由于官网被墙，可以采用下载插件离线包本地安装的方式，地址为，[ADT Plugin](http://www.androiddevtools.cn))，Maven支持插件，Gradle支持插件、Python插件及Ruby插件。

### eclim

[eclim](http://eclim.org/)

[eclim 下载](http://sourceforge.net/projects/eclim/)，当前最新稳定版本是2.5.0，使用如下使用安装，

    java -jar eclim_2.5.0.jar

### emacs-eclim

[emacs-eclim](https://github.com/senny/emacs-eclim)

快捷键

* 打开.spacemacs SPC f e d
* EVAL .spacemacs SPC f e R

* 最近打开文件 SPC f r

* Helm-swoop开始(区分范围) SPC s S
* Helm-swoop开始(不区分范围) SPC s s
* Helm-swoop 进入编辑模式 C-c C-e
* Helm-resume SPC h l

* 查找工程中的文件 SPC p f
* 查找工程中的目录 SPC p d
* 打开工程的根目录 SPC p D

* 转到定义 SPC m g g
* 转到当前buffer内的函数/方法 SPC s l
* 显示错误 SPC m e b
* 跳到后一个错误 SPC m e n
* 跳到前一个错误 SPC m e p

## 文本操作

* copy-and-comment-lines，SPC c y
* comment-or-uncomment-line，SPC c l
* kill-line，C+k
* avy-copy-line
* move down a line of text (enter micro-state)，SPC x J，M+UpArrow
* move up a line of text (enter micro-state)，SPC x K，M+DownArrow
* swap (transpose) the current line with the previous one，SPC x t l

## frame 操作

* 最大化frame，SPC T M
* 全屏 frame, SPC T F
* 切换透明效果，SPC T T
* 切换末尾的波浪线(仿VIM)，SPC T f
* 切theme，SPC T h

# buffer 操作

* next-buffer，下一个窗口，C+x C+->
* previous-buffer，前一个buffer，C+x C+<-
* 只读模式，SPC b w
* 将当前buffer内容替换为剪贴板内容，SPC b P
* 复制当前buffer内容到剪贴板，SPC b Y
* 显示当前打开的buffer，SPC b b
* 跳转到spacemace Home buffer，SPC b h
* kill other buffers，SPC b K

## window操作

* other-window，跳转到下一个窗口，C+x o
* split-window-below，纵向分割窗口，C+x 2
* split-window-right，横向分割窗口，C+x 3
* spacemacs/toggle-maximize-buffer，最大化当前窗口，C+x 1
* delete-window，关闭当前窗口，C+x 0
* enlarge-window，扩大当前窗口纵向，C-x ^
* shrink-window，缩小当前窗口纵向
* enlarge-window-horizontally，扩大当前窗口横向，C+x }
* shrink-window-horizontally，缩小当前窗口横向，C+x {
* balance-windows，使各个窗口一样大，C+x +
* shrink-window-if-larger-than-buffer，使窗口大小与内容匹配，C+x -
* spacemacs/rotate-window，调换窗口，SPC w R
* ace-window，切换窗口，SPC w SPC
* spacemacs窗口操作前缀，SPC w

## frame 操作

* make-frame，C-x 5 2，make-frame，Emacs will make a new frame containing the current buffer
* other-frame，C-x 5 o，move between Frames
* delete-frame，C-x 5 0，delete frame
* ido-find-file-other-fram，C-x 5 f，打开一个文件在新的frame
* switch-to-buffer-other-frame，C-x 5 b，swith to buffer other frame

## Markdown

* 中文跟英文混合的时候，中文和英文之间会自动加上空格。这个实在是太方便了！

