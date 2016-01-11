---
layout: post
title: "在 Org mode 环境中执行 Ruby"
date: 2016-01-09 23:59:59 +0800
comments: true
sharing: false
categories: emacs "Org mode" ruby
---

# 什么是 Org mode

按照官网 [http://orgmode.org/](http://orgmode.org/) 的说法，

> Org mode is for keeping notes, maintaining TODO lists, planning projects, and authoring documents with a fast and effective plain-text system.

简单来说，Org mode 就是用来记笔记、维护待办列表、做计划以及写作文档的快速高效的纯文本系统。在一个 org 文档中，所有的信息都是以纯文本来存储的，具有良好的可读性及跨平台性。org文档的设计实际上是独立于emacs的，因为它是纯文本，只要遵从了它的一些简单语法规则，可以用任何文本编辑器来修改。

尽管 org 文档的设计可以与 emacs 无关，但其具体实现和运用来说，脱离了 emacs 这个环境，它的实际使用的效能可能会降低 90%，emacs 为 org 格式的文档提供了编辑、展现、发布、转换等相关的功能，使得两者相得益彰。

基于literate programming 的概念，org 提供了对源代码的相应操作支持。可以在 org 文档中支持多种编程语言的编辑、运行、代码块调用执行、运行结果导出等。它支持的语言为 [http://orgmode.org/manual/Languages.html#Languages](http://orgmode.org/manual/Languages.html#Languages)。

本文写作时 Org mode的版本为8.3.2。

# 配置 Org mode

## 添加 Org mode 支持的语言

缺省 Org mode只支持执行 elisp，需要在 .emacs 中加入如下配置来支持 ruby，

    (org-babel-do-load-languages
          'org-babel-load-languages
          '((emacs-lisp . t)
            (ruby . t)))

需要注意的是，在 org 文档中执行代码是有安全风险的，要注意鉴别要执行的代码。

# 代码块格式

代码块是一个类似如下形式的文本块，

    #+NAME: <name>
    #+CAPTION: <caption>
    #+BEGIN_SRC <language> <switches> <header arguments>
       <body>
    #+END_SRC

在 org 文档中，以 `#=` 开头的部分为文档或代码块的元数据，其中 `#+NAME:` 用来标识这段代码块，以便在其他地方引用或调用。`#+CAPTION:` 为导出整个 org 文档时，在导出的文档中(HTML或pdf等格式)，这个代码块的标题。`<language>` 代表支持的语言标识符。`<switches>` 是用来控制代码块导出到文档的参数。`<header arguments>` 用来控制literate programming中的相关参数和代码执行参数。`<header arguments>` 的参数比较多，也比较复杂，可以参考 [http://orgmode.org/manual/Header-arguments.html#Header-arguments](http://orgmode.org/manual/Header-arguments.html#Header-arguments)。

另外，由于需要经常输入代码块，可以在 emacs 中先输入 `<`，再输入 `s`，再按TAB键来生成代码块模板。有以下快捷模板，

    s	#+BEGIN_SRC ... #+END_SRC 
    e	#+BEGIN_EXAMPLE ... #+END_EXAMPLE
    q	#+BEGIN_QUOTE ... #+END_QUOTE 
    v	#+BEGIN_VERSE ... #+END_VERSE 
    c	#+BEGIN_CENTER ... #+END_CENTER 
    l	#+BEGIN_LaTeX ... #+END_LaTeX 
    L	#+LaTeX: 
    h	#+BEGIN_HTML ... #+END_HTML 
    H	#+HTML: 
    a	#+BEGIN_ASCII ... #+END_ASCII 
    A	#+ASCII: 
    i	#+INDEX: line 
    I	#+INCLUDE: line 
    
    
# 添加一个Ruby代码块

在 org 文档中添加一个 ruby 代码块如下，

    #+NAME: snippet:basic_ruby
    #+CAPTION: Ruby 基本语句
    #+BEGIN_SRC ruby -n -i -r :exports code
    puts 'hello ruby' # (ref:puts)
    80.times {puts '*'} # (ref:loop)
    '*' * 80
    # this is single comment  # (ref:singlelinecomment)
    =begin
    multi line
      comment
    =end
    #+END_SRC
    
    这行是[[(puts)][打印语句]]，这行是[[(singlelinecomment)][单行注释]]，这行是[[(loop)][循环]]。
        
ruby 代码为 `#+BEGIN_SRC` 到 `#END_SRC` 之间的文本。

## 编辑

光标移动到代码块中，按键 `C-c '`，emacs 会自动弹出一个临时 buffer，这个 buffer 已经处于 ruby 相关的major mode，内容为代码块中的 ruby 代码。在这个窗口中可以自由的编写 ruby 代码。编写过程中，可以按 `C-x C-s`，将代码同步到 org 文档中。也可以完成编辑后，再按 `C-c '` 关闭这个临时 buffer，并将内容同步到 org 文档中的代码块中。

前面的三个选项中，`-i` 表示在临时 buffer 中编辑源代码时，保持缩进为代码块中的样子，而不要根据ruby的一般缩进改变代码，比如，如果不加 `-i` 选项，则临时 buffer 会在各行代码前面加上2个空格的缩进。

在临时 buffer 中，还可以使用 `C-c l` 来对各行添加引用，会生成格式如 `(ref:xxx)` 的引用，`xxx` 为 emacs 会提示你输出的引用名。这些引用会保存起来，在后面的 org 文档写作中，可用使用 `C-c C-l` 来方便地插入这些引用。

## 执行

在代码块中按 `C-c C-c`，emacs 提示后，可以执行代码。代码执行的结果会添加到到着代码块的下面，例如，上面 ruby 代码块的输出结果为，

    #+RESULTS:
    : ********************************************************************************

## 导出

Org mode 提供了将 org 文档导出为其他格式的能力。代码块也可以在导出时加以控制。选项 `-n` 表示，在导出代码块时，会在各行加上行号。 `-r` 表示，在导出时去掉代码行引用。在上面的代码块中，`(ref:puts)` 为该代码行的一个引用，注意，引用名为 `puts`，在后面的 org 文档内容中，可以链接到这行代码，在文档用来指示代码时，非常有用。

导出时也可以决定是否执行代码块后再导出。由 header arguments `:exports code` 决定，:export的值有4个，为，code、results、both、none，分别表示只导出代码(导出时不执行)，只导出执行结果(导出时要执行)、导出代码和执行结果(导出时要执行)及不导出代码和执行结果(导出时不执行代码)。
