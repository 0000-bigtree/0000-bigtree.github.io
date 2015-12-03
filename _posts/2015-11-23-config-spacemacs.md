---
layout: post
title: "配置 spacemacs"
date: 2015-11-23 18:00:00 +0800
comments: true
sharing: false
categories: emacs
---
    
# Emacs and spacemacs

程序员手中的文本编辑器，到最后阶段，一般都会选择 [Emacs](http://www.gnu.org/software/emacs/emacs.html) 或 [vim](http://www.vim.org/)，我选择的是 emacs。

不过，对 emacs 的扩展和定制向来不是简单容易的，emacs 如果没有扩展和定制来增强功能、贴合你的需求，使用起来，它的体验就会比较差，就像满怀兴奋地以为得到了一件旷世神器，当你使用它时，却是满心失望，觉得它不过如此，见面不如闻名。其中的原因是，emacs 是需要扩展和定制的，要在实践中不断地使用它，熟悉它的各个功能，探究新的使用方式，找到更高效的、满足于你实际需要的、符合你使用习惯的方式，这样才能得心应手，纵横驰骋。这个过程，与古代的工匠不断打磨自己的工具类似。

在遥远的蛮荒时代(其时也就 7、8 年前)，打磨 emacs 的方法是在网上一条条地找配置或技巧，怎么样缺省显示行号，怎么样在 frame 标题栏显示当前 buffer 的文件全路径等等。每搜集到一条相关的 elisp 代码，把它放入 $HOME/.emacs 中，再验证和调整。这个过程麻烦又琐碎，上手缓慢，效率奇低。另外一种方法是是使用大牛的 .emacs 文件，但是这些大牛的想法，你知道的，高山仰止，又懒得花时间对你循循善诱，即使费心千辛万苦让这个 .emacs 在你的环境中工作正常了，你可能还是云里雾里，也难以调整它来符合你的需求。

emacs 的功能众多和扩展定制麻烦，导致上手困难，即使对于程序员，也要花费大量的时间来学习、调整和掌握它。这方面，不得不说，Sublime、Atom、JEdit、UltraEdit 要做得好很多，很多功能是开箱即用的，相对于 emacs，需要配置的地方要少得多。但自从 emacs 24 提供了包管理工具 package.el 后，可以通过 ELPA(Emacs Lisp Package Archive) 网站来安装扩展包，已经是非常轻松的事。ELPA 对于 emacs 社区来说，也是极为重要的一件事，它将社区中庞大的的功能扩展在同一个地方，以相同的方式组织起来，提供这些扩展的各个版本及它们各自的依赖。emacs 可以访问 ELPA 网站，进行安装、升级功能扩展包。功能扩展的发布、升级及 emacs 功能扩展搜索、安装、升级这些流程可以依托 ELPA 网站来进行完整流畅的循环，促进了 emacs 的扩展质量提高，整个 emacs 社区也更加繁荣。

对于定制这个问题，Github 这类网站让程序员协作变得更加方便和高效，可以集中更多志趣相投的程序员来做同样的事情。随着开发力量的增强和其他项目触发的一些灵感 (如 [Oh My Zsh](http://ohmyz.sh/))，对 emacs 的定制已经不再局限于折腾 .emacs 文件这样的小规模代码编写了，对 emacs 的定制和二次开发已经变成代码行可观的项目，这些项目借鉴了原来的一些经验和成果，把它们集成和包装起来，辅助丰富的文档、巧妙的设计、稳定可靠的质量及迅速的使用反馈，让初学者打磨 emacs 的效率大大提高了，体验也更加良好。这些项目主要有：[Emacs Starter Kit](https://github.com/technomancy/emacs-starter-kit)、[emacs.d](https://github.com/purcell/emacs.d)、[Emacs Prelude](https://github.com/bbatsov/prelude)、[spacemacs](https://github.com/syl20bnr/spacemacs)等。

前面用过一段时间 Emacs Prelude，后面看到 spacemacs 之后，就转了过来。主要原因是：

* spacemacs 的开发社区更活跃。gihub 上的 start也最多，的确有聚集效应。
* 文档很丰富，方方面面都详细讲到。
* 设计比较规范和完整，统一的快捷键，约定惯例，用户扩展方式等等。尤其它的 layer 这个抽象感觉很赞，干净。
* 已经定义好的 layer 也多，满足了需要，开箱即用。
* 版本更新很快，bug 响应分分钟。
* 外观漂亮。
* 高度兼容 vim 操作习惯。

这里有一个技巧收集网站，[Spacemacs Rocks](http://spacemacs.brianthicks.com/)。

# Windows 版本及配置

## 安装 Emacs

当前的官方最新版本是 [24.5](https://ftp.gnu.org/gnu/emacs/windows/emacs-24.5-bin-i686-mingw32.zip)，
不过，spacemacs 推荐的是 64 位最新编译版本[here](http://emacsbinw64.sourceforge.net/)，这个是非官方的，由于这个是由最新源代码编译的，它的版本号是 25.0.50.1。

解压后，目录结果如下所示，这样的结构便于版本升级，在各个操作系统下使用统一的配置文件，

    C:\coder
      emacs\
        25.0.50.1-x86_64-w64-mingw32\
        home\
        
## 安装 spacemacs

    cd c:\coder\emacs\home
    git clone https://github.com/syl20bnr/spacemacs.git .emacs.d

## 安装 The Platinum Searcher

从这里取对应操作系统的版本来安装，[The Platinum Searcher](https://github.com/monochromegane/the_platinum_searcher)，pt 有 Windows、Linux、OSX 上现成的版本，相对于 ag、grep，在 Windows 上不需要安装 MinGW 或 Cygiwn，比较方便。

emacs 中 [pt.el](https://github.com/bling/pt.el)，主要用来做代码搜索的。这个会由 spacemacs 自动安装。

## 启动 emacs 的批处理文件

在 emacs 目录下新建 emacs.bat 文件，内容如下，

    @SETLOCAL
    @CD /D C:\coder\emacs\home
    @SET HOME=%CD%
    @START %HOME%\..\25.0.50.1-x86_64-w64-mingw32\bin\runemacs.exe --no-splash -mm %*
    @ENDLOCAL

emacs 会读取 HOME 系统变量，作为自己的主目录。启动 emacs 时，使用 runemacs.exe，不会在 emacs 窗口后面出现控制台窗口。`--no-splash` 参数指定无启动闪屏，`--mm` 参数指定窗口最大化。`%*` 是传入批处理的命令行参数。

编辑 emacs 目录下 .gitignore，过滤不需要放入 git 管理的文件和目录，

    25.0.50.1-x86_64-w64-mingw32
    home/.backups
    home/.emacs.d
    home/.session

目录结构变成，

    C:\coder
      emacs\
        25.0.50.1-x86_64-w64-mingw32\
        home\
          .emacs.d\
        .gitignore

由于在 emacs.bat 中定义了 HOME 变量的值是 `C:\coder\emacs\home`，则 emacs 会把这个路径认为是用户主目录，相关的文件都会存储到该目录中。

## Source Code Pro 字体

[Source Code Pro](https://github.com/adobe-fonts/source-code-pro) 是 Adobe 开源的等宽字体，spacemacs 缺省使用了它。大家都说好，但我觉得在 Windows 上效果不怎么样，在 OSX 上，看起来的确是不错的。

Github上的README已经有很详细的安装方法，按照文档所述的步骤，下载对应操作系统的安装文件安装即可。

## .spacemacs 文件

这个文件是 spacemacs 的主要配置文件，相关的 spacemacs 都可以在这里找到。spacemacs 第一次启动时，会按照模板，自动在 $HOME 中生成这个文件。并且会提示你选择 vim 风格或 emacs 风格。vim 风格会使用与 vim 相似的操作习惯。如果使用了 vim 风格，则其 Leader key 会是 SPC，即空格键，而选择了 emacs 风格，其 Leader key 会是 M-m。Leader key 是指按键序列中的第一次按键。其实在 .spacemacs 中需要配置的地方不多，主要有：

### 配置需要的 layer 及其变量

    ...
    (defun dotspacemacs/layers ()
    ...
    ;; List of configuration layers to load. If it is the symbol `all' instead
    ;; of a list then all discovered layers will be installed.
    dotspacemacs-configuration-layers
    '(
      ;; ----------------------------------------------------------------
      ;; Example of useful layers you may want to use right away.
      ;; Uncomment some layer names and press <SPC f e R> (Vim style) or
      ;; <M-m f e R> (Emacs style) to install them.
      ;; ----------------------------------------------------------------
      chinese
      smex
      (ranger :variables
              ranger-show-preview t)
      ;; better-defaults
      git
      (shell :variables
             shell-default-height 30
             shell-default-position 'bottom)
     ...

在上面的代码中配置需要的 layer，并且还可以配置 layer 变量，比如，下面的代码展示 ranger layer 需要的一个变量 ranger-show-preview，将其配置为 t：

      (ranger :variables
              ranger-show-preview t)

### 配置额外安装的扩展包

    ...
    (defun dotspacemacs/layers ()
    ...
    ;; List of additional packages that will be installed without being
    ;; wrapped in a layer. If you need some configuration for these
    ;; packages then consider to create a layer, you can also put the
    ;; configuration in `dotspacemacs/config'.
    dotspacemacs-additional-packages '(
                                       monokai-theme
                                       )
    ...

在上面的代码中，额外安装了 monokai-theme 这个包，这个包未在 spacemacs 的缺省需要安装包中，也未在已经配置的 layer 中，所以需要额外安装。

### themes

    ...
    (defun dotspacemacs/init ()
    ...
    ;; List of themes, the first of the list is loaded when spacemacs starts.
    ;; Press <SPC> T n to cycle to the next theme in the list (works great
    ;; with 2 themes variants, one dark and one light)
    dotspacemacs-themes '(monokai
                          spacemacs-dark
                          spacemacs-light
                          solarized-light
                          solarized-dark
                          leuven
                          zenburn)
    ...

配置可用 M-m T n 循环切换的 theme。

### 字体

    ...
    (defun dotspacemacs/init ()
    ...
    ;; Default font. `powerline-scale' allows to quickly tweak the mode-line
    ;; size to make separators look not too crappy.
    dotspacemacs-default-font '("Source Code Pro"
                                :size 18
                                :weight normal
                                :width normal
                                :powerline-scale 1.1)
     ...

### 自定义初始化

    ...
    (defun dotspacemacs/user-config ()
      "Configuration function for user code.
     This function is called at the very end of Spacemacs initialization after
    layers configuration. You are free to put any user code."
      ;; 显示行号
      (add-hook 'find-file-hooks (lambda () (linum-mode 1)))
      ...

这个地方的代码可以用来配置各个扩展包的特性。如果这些配置没有包含在 spacemace 中的话。

## 自定义层

# OSX 版本及配置

# Linux 版本及配置

# 通用特性

## 文本操作

### 行

* copy-and-comment-lines，SPC c y
* comment-or-uncomment-line，SPC c l
* kill-line，C+k
* avy-copy-line
* move down a line of text (enter micro-state)，SPC x J，M+UpArrow
* move up a line of text (enter micro-state)，SPC x K，M+DownArrow
* swap (transpose) the current line with the previous one，SPC x t l
    
### 矩形区域

* 填充字符，C-x r t

## buffer 操作

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

## 其他

* 最大化frame，SPC T M
* 全屏 frame, SPC T F
* 切换透明效果，SPC T T
* 切换末尾的波浪线(仿VIM)，SPC T f
* 切换theme，SPC T h
* 循环切换 theme，SPC T n

# Java

## JDEE

[JDEE](https://github.com/jdee-emacs/jdee)，以前比较好的一套插件，但很长时间没有更新，把代码库迁移到Github后，现在处于reborn状态，但目前进展比较缓慢，按照它的RoadMap，是要支持Maven、Gradle的，保留期待。

## emacs-eclim

可以看 [eclim 介绍](http://www.emacswiki.org/emacs/EmacsEclim) 文章。

项目地址为，[eclim github](https://github.com/senny/emacs-eclim)。

### eclipse

下载最新的 4.5.1版本，地址为[http://www.eclipse.org/downloads/](http://www.eclipse.org/downloads/)，注意根据你操作系统的JDK 版本选择对应的64位或32位Eclipse版本。

安装必要的插件，ADT(由于官网被墙，可以采用下载插件离线包本地安装的方式，地址为，[ADT Plugin](http://www.androiddevtools.cn))，Maven支持插件，Gradle支持插件、Python插件及Ruby插件。

### eclim

[eclim](http://eclim.org/)

[eclim 下载](http://sourceforge.net/projects/eclim/)，当前最新稳定版本是2.5.0，使用如下使用安装，

    java -jar eclim_2.5.0.jar

### emacs-eclim

[emacs-eclim](https://github.com/senny/emacs-eclim)
    
emacs-clim 快捷键

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

# Markdown

# Calc

打开 calc，C-x * c；打开 calc 及在线帮助，C-x * t。

`+`，加号；`-`，减号；`*`，乘号；`/`，除号；`%`，取余，比如 %5=1, 30%4=2；`&`，取倒数；`^`，幂运算，当然也可以用做开方运算，比如 4^0.5=2。

使用逆波兰式来计算。不使用逆波兰式，需要命令 `\``。

d0，切换为 10 进制；d6，切换为 16 进制；d2，切换为二进制；d8，切换为八进制。

缺省输入的数都是 10 进制，如果输入的数是当前数制，使用 `#` 开头。

[手册](http://www.delorie.com/gnu/docs/calc/calc.html)  
[说明](http://emacser.com/calc.htm)

