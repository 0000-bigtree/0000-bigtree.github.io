---
layout: post
title: "用 Graphviz 来绘图"
date: 2016-04-08 23:00:00 +0800
comments: true
sharing: false
categories: graphviz
---
# 简介
[Graphviz](http://www.graphviz.org/) 是一个绘图工具集，可以用称之为 [The DOT Language](http://www.graphviz.org/content/dot-language) 的 DSL 来绘图。用 dot 写好脚本之后，使用不同的布局引擎来对脚本解析，生成图片，支持 PNG、PDF 等格式。Graphviz 有好几个布局引擎，一般使用的有 dot (有向图)和 circo(环形布局)，其他的较少使用。

Graphviz 包含 3 种图形元素，图(graph)，节点(node)和边(edge)。每个元素都可以具有各自的属性，用来定义字体，样式，颜色，形状等。

这里有一个 dot 用户指南， [Drawing graphs with dot](http://www.graphviz.org/pdf/dotguide.pdf)。

## 基础

```
// structure.gv
digraph G { // 图的名称为 G，digraph 表示这是一个有向图
    main -> parse -> execute; // 节点及其指向的节点，表示 main 节点指向 parse 节点，而 parse 节点又指向 execute 节点
    main -> init;
    main -> cleanup;
    execute -> make_string;
    execute -> printf
    init -> make_string;
    main -> printf;
    execute -> compare;
}
```

graph 目前就只有 1 个，即 G，node 共 8 个，edge 表示了节点之间的指向。使用命令 `dot -Tpng structure.gv  -o structure.png` 生成的图如下所示，

![structure.png](/resources/img/206-04-08-graphviz-syntax/structure.png)

从上个脚本可以看出大致结构，现在做一些改进，定义一些属性，主要会影响到 node 和 edge，

```
// exstructure.gv
digraph G {
    node [shape="record"]; // 定义节点的开关为矩形，对所有节点生效
    edge [style="dashed"]; // 定义边，及指向的形状为虚线，对所有边生效

    main [style="filled", color="black", fillcolor="chartreuse"]; // 指定 main 节点的属性，包括填充及字体颜色
    main -> parse -> execute;
    main -> init;
    main -> cleanup;
    execute -> make_string;
    execute -> printf
    init -> make_string;
    main -> printf;
    execute -> compare [color="red"]; // 节点 execute 到 compare 的线为红色
}
```

结果如下图所示，

![exstructure.png](/resources/img/206-04-08-graphviz-syntax/exstructure.png)

前面都使用的是 dot 布局，换成 circo 布局如下图所示，命令为 `circo -Tpng exstructure.gv  -o circo-structure.png`，

![circo-structure.png](/resources/img/206-04-08-graphviz-syntax/circo-structure.png)

## 子图

子图简单来说，就是把图里面的节点分组，使整个图形的各个部分看起来更加清晰。

```
digraph G {
    subgraph cluster0 { // 子图定义，名称须以 cluster 开头
        node [style=filled,color=white]; // 只针对整个子图的属性定义
        style=filled;
        color=lightgrey;
        a0 -> a1 -> a2 -> a3;
        label = "process #1";
    }
    
    subgraph cluster1 {
        node [style=filled];
        b0 -> b1 -> b2 -> b3;
        label = "process #2";
        color=blue
    }
    
    start -> a0;
    start -> b0;
    a1 -> b3;
    b2 -> a3;
    a3 -> a0;
    a3 -> end;
    b3 -> end;
    start [shape=Mdiamond];
    end [shape=Msquare];
}
```

结果如下图所示，

![cluster.png](/resources/img/206-04-08-graphviz-syntax/cluster.png)

# 在 Org mode 中集成
看[这里](http://orgmode.org/worg/org-contrib/babel/languages/ob-doc-dot.html)的介绍，注意 :cmd 参数，如果需要使用非 dot 布局器，则需要给这个参数指定合适的布局器命令，如果使用 dot 布局器，可以省略这个参数。如果命令有参数，用 :cmdline 来指定。

```
#+BEGIN_SRC dot :cmd dot :file test-dot.png :exports results
digraph G {
main -> parse -> execute;
main->init;
main->cleanuy;
execute->make_string;
execute->printf
init->make_string;
main->printf
execute->compare
}
#+END_SRC
```

Orgmode 也提供了一些高级功能，可以由另外一个源代码块来生成输出，作为 dot 代码块的输入。

以 emacs-lisp 产生输出，即生成 Graphviz dot 脚本，

```
#+name: make-dot
#+BEGIN_SRC emacs-lisp :var table=dot-eg-table :results output :exports none
  (mapcar #'(lambda (x)
              (princ (format "%s [label =\"%s\", shape = \"box\"];\n"
                             (first x) (second x)))) table)
              (princ (format "%s -- %s;\n" (first (first table)) (first (second table))))
#+END_SRC
```

引用 emacs-lisp 代码块产生的输出($input 变量接收)作为输入，

```
#+BEGIN_SRC dot :file images/test-dot.png :var input=make-dot :exports results
graph {
 $input
}
#+END_SRC
```

# 在 Ruby 中集成

由 [Ruby-Graphviz](https://github.com/glejeune/Ruby-Graphviz) gem 支持，可以用 Ruby 语言 DSL 来生成 Graphviz dot 脚本，的确是绕了一层，但是 Ruby 是灵活的通用编程语言，在它的帮助下，可以动态地生成 dot 脚本，也就是动态生成图形。另外，Ruby-Graphviz 还支持 [GraphML](http://graphml.graphdrawing.org/)，GraphML 以 XML 来描述图形。这里有[一篇 GraphML 简单介绍](http://my.oschina.net/sulliy/blog/209077)，感觉 XML 挺复杂的。

提供了一些命令行工具，暂时只列两个，

  * git2gv 

    在 git 仓库目录中使用，用来生成 commit 图。例如，`git2gv -Tpng -o commits.png`
    
  * dot2ruby
  
    把 dot 脚本转换为 Ruby 脚本。
    
# 在 Asciidoc 中集成

由 [Asciidoctor Diagram](https://github.com/asciidoctor/asciidoctor-diagram) gem 来提供支持。与 Org mode 类似，把 Graphviz got 绘制脚本嵌入到 Asciidoc 文档中。由 Asciidocor Diagram 自动处理。

# 其他类似的绘图工具
[Ditaa](http://ditaa.sourceforge.net/) 是一个用 Java 开发的小工具，可以将 ASCII 图转成漂亮的图片。

[BlockDiag, SeqDiag, ActDiag, NwDiag](http://blockdiag.com/) 是 Python 写的工具，用来绘制框图、序列图、活动图和网络图，跟 PlantUML 的功能有点重合，但是 [nwdiag(simple network-diagram image generators)](http://blockdiag.com/en/nwdiag/index.html)，可以用来绘制网络图和网络包结构。

[Mermaid](http://knsv.github.io/mermaid/)，是 JavaScript 编写的，跟 Graphviz 功能有重合，但可以绘制 Gant(甘特图) 图。有些功能看起来比 Graphviz 简洁。效果也不错。

[Shaape](https://github.com/christiangoltz/shaape)，Python 写的，跟 Ditaa 类似，将 ASCII 转成图片，功能更强大，但感觉也挺复杂。

[PlantUML](http://plantuml.sourceforge.net/)，用 DSL 来画 UML 图。实际内部也用了 Graphviz。

[WaveDrom](http://wavedrom.com/)，用来做数字时序图的，这个估计用不上。

# 参考

[使用graphviz绘制流程图（2015版）](http://icodeit.org/2015/11/using-graphviz-drawing/)

[使用 Graphviz 生成自动化系统图](https://www.ibm.com/developerworks/cn/aix/library/au-aix-graphviz/)

[用 Graphviz 可视化函数调用](https://www.ibm.com/developerworks/cn/linux/l-graphvis/)

[使用 Ruby 和 Twitter 进行数据挖掘](http://www.ibm.com/developerworks/cn/opensource/os-dataminingrubytwitter/)，里面用了 circo 布局来生成 Twitter 关注者图表。

[学习用 doxygen 生成源码文档](https://www.ibm.com/developerworks/cn/aix/library/au-learningdoxygen/)，Doxygen 内部利用 Graphviz 生成相关 UML 图。

[探究 Web 页面之间的可视化关系](https://www.ibm.com/developerworks/cn/opensource/os-graphviz/)，用来 Graphviz 来生成页面之间的可视关系。
