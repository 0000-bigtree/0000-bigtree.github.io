---
layout: post
title: "Jython 安装配置指南 for Windows"
date: 2014-01-10 10:33
comments: true
sharing: false
categories: Python Jython Java
---
Jython 是 Python 在 JVM上 的实现。目前稳定版为 2.5.3，兼容 CPython 2.5。


# 1. 安装 JDK

略。

# 2. 安装 Jython

Jython 的官网为 [http://www.jython.org](http://www.jython.org)，当前的最新版本为 2.5.4rc1。
Jython 提供了三种安装版本，分别是，

* jython-installer-x.x.x.jar

提供了一个图形安装界面和命令行安装界面，一般使用图形安装方式，使用命令

java -jar jython-installer-x.x.x.jar

运行图形界面安装程序，前面几个步骤非常简单，选择“Next”即可，到选择安装类型时，如下图所示，

![Jython安装类型选择](/resources/img/2014-01-10-windowsxia-jythonan-zhuang-zhi-nan/install_type.png)

有五个选项，All、Standard、Minimum、Standalone、Custom，All 包含了源代码、Python库、文档、示例；Standard 未包含源代码；Mininum 仅包含了核心实现；Standalone 包含了核心实现及Python库；Custom 可以定制你要安装的内容。

一般情况下，选择 Standard 或 All，方便在本地查看文档和示例代码。如果选择 All，还可以查看源代码。

* jython-standalone-x.x.x.jar

无需特别的安装步骤，直接下载拷贝 jar 文件到指定位置，它将 Jython 实现、依赖的 Java 库及 Python
标准库一起打包为一个 jar 文件，使用时非常方便，运行 Phtyon 代码时，使用如下命令直接运行即可，

java -jar jython-standalone-x.x.x.jar script.py

* jython-x.x.x.jar

跟 jython-standalone-x.x.x.jar 的区别是，未将 Python 库文件(.py)一起打包在 jar 中。

# 3. 配置 Jython

* 配置环境变量 JYTHON_HOME

Jython 的安装目录，如下图所示

![Jythoh Home](/resources/img/2014-01-10-windowsxia-jythonan-zhuang-zhi-nan/jython_home.png)

* 配置环境变量 JYTHONPATH

Jython 模块(库)的搜索目录。

* 配置环境变量 JYTHON_OPTS

Jython 解释器的缺省选项，运行 Jython 解释器时，会自动加上这些选项。

* 配置环境变量 Path

将%JYTHON_HOME%\bin 加到可执行文件搜索路径中，以“;”分隔，如下图所示，

![Executable Path](/resources/img/2014-01-10-windowsxia-jythonan-zhuang-zhi-nan/jython_path.png)

# 4. 验证安装

启动 Windows 命令行，输入 jython，即可进入 python 控制台，如下图所示，

![Jython Console](/resources/img/2014-01-10-windowsxia-jythonan-zhuang-zhi-nan/console.png)

# 5. 参考文档

官方安装指南 [https://wiki.python.org/jython/InstallationInstructions](https://wiki.python.org/jython/InstallationInstructions)

Jython 2.5.2 文档 [http://www.jython.org/docs/index.html](http://www.jython.org/docs/index.html)

- - -

# 修改历史
1.0.0，bigtree，2014年1月10日  
created  

1.0.0，bigtree，2014年1月13日  
release

1.0.0，bigtree，2015年5月25日  
迁移到 jekyll，符合 GFM 的语法
