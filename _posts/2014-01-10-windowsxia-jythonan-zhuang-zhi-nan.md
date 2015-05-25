---
layout: post
title: "Jython安装配置指南for Windows"
date: 2014-01-10 10:33
comments: true
sharing: false
categories: Python Jython Java
---
Jython是Python在JVM上的实现。目前稳定版为2.5.3，兼容CPython 2.5。

<!-- more -->
#### 1. 安装JDK

略。

#### 2. 安装Jython

Jython的官网为 [http://www.jython.org](http://www.jython.org)，当前的最新版本为2.5.4rc1。
Jython提供了三种安装版本，分别是，

* jython-installer-x.x.x.jar

提供了一个图形安装界面和命令行安装界面，一般使用图形安装方式，使用命令

java -jar jython-installer-x.x.x.jar

运行图形界面安装程序，前面几个步骤非常简单，选择“Next”即可，到选择安装类型时，如下图所示，

{% img center /images/2014-01-10-windowsxia-jythonan-zhuang-zhi-nan/install_type.png "Jython安装类型选择"%}

有五个选项，All、Standard、Minimum、Standalone、Custom，All包含了源代码、Python库、文档、示例；Standard未包含源代码；Mininum仅包含了核心实现；Standalone包含了核心实现及Python库；Custom可以定制你要安装的内容。

一般情况下，选择Standard或All，方便在本地查看文档和示例代码。如果选择All，还可以查看源代码。

* jython-standalone-x.x.x.jar

无需特别的安装步骤，直接下载拷贝jar文件到指定位置，它将Jython实现、依赖的Java库及Python标准库一起打包为一个jar文件，使用时非常方便，运行Phtyon代码时，使用如下命令直接运行即可，

java -jar jython-standalone-x.x.x.jar script.py

* jython-x.x.x.jar

跟jython-standalone-x.x.x.jar的区别是，未将Python库文件(.py)一起打包在jar中。

#### 3. 配置Jython

* 配置环境变量JYTHON_HOME

Jython的安装目录，如下图所示

{% img center /images/2014-01-10-windowsxia-jythonan-zhuang-zhi-nan/jython_home.png "Jythoh Home"%}

* 配置环境变量JYTHONPATH

Jython模块(库)的搜索目录。

* 配置环境变量JYTHON_OPTS

Jython解释器的缺省选项，运行Jython解释器时，会自动加上这些选项。

* 配置环境变量Path

将%JYTHON_HOME%\bin加到可执行文件搜索路径中，以“;”分隔，如下图所示，

{% img center /images/2014-01-10-windowsxia-jythonan-zhuang-zhi-nan/jython_path.png "Executable Path"%}

#### 4. 验证安装

启动Windows命令行，输入jython，即可进入python控制台，如下图所示，

{% img center /images/2014-01-10-windowsxia-jythonan-zhuang-zhi-nan/console.png "Jython Console"%}

#### 5. 参考文档

官方安装指南 [https://wiki.python.org/jython/InstallationInstructions](https://wiki.python.org/jython/InstallationInstructions)

Jython 2.5.2文档 [http://www.jython.org/docs/index.html](http://www.jython.org/docs/index.html)

- - -
#### 修改历史
1.0.0，bigtree，2014年1月10日  
created  

1.0.0，bigtree，2014年1月13日  
release

