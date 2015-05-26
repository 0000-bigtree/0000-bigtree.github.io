---
layout: post
title: "制作 Oracle SQL*Plus 的 Wndows 32位 Portable 版本"
date: 2013-03-22 00:04
comments: true
sharing: false
categories: Database Oracle SQL*Plus
---
# Oracle SQL*Plus 简介
SQL*Plus 是 Oracle 数据库的客户端连接工具，操作界面非常简洁，没有复杂的图形用户界面，仅提供命令行类似的操作界面，
但特性丰富且功能强大。SQL\*Plus 之于 Oracle 数据库，类似于 MySQL 客户端(mysql)之于 MySQL 数据库，当然，SQL\*Plus 与 mysql 在功能上没有可比性，两者不是同一个级别的。在不同的平台下，
Oracle 提供了 Oracle 数据库的可执行安装程序，SQL\*Plus 作为其中的一个组件，可单独安装或与 Oracle 数据库一起安装，
但安装过程复杂，耗时较长。


本文说明了制作一个 Windows 32 位操作系统环境下、免安装的、SQL\*Plus 版本，使用的 SQL\*Plus 的版本号为 11.2.0.3.0。
这样，当在多台机器上需要使用 SQL\*Plus 时，直接拷贝使用，免去安装过程的麻烦。在其他版本的 Windows 操作系统环境下，
SQL\*Plus 免安装版本的制作过程类似，不再赘述。另外，限于时间，**本文测试(实验)的 Window 32
位操作系统环境为 Windows XP SP3，没有在其他 Window 32 位操作系统环境下(如 32 位 Windows Vista、32 位 Windows 7等 )做测试，但一般情况下，在这些 Windows 32 位操作系统环境中，也是适用的。**

# 制作步骤 1，去 Oracle 官方网站下载需要的软件
去网址 [http://www.oracle.com/technetwork/topics/winsoft-085727.html](http://www.oracle.com/technetwork/topics/winsoft-085727.html) 下载
如下图标识的两个软件包，分别为 `instantclient-basic-nt-11.2.0.3.0.zip` 和 `instantclient-sqlplus-nt-11.2.0.3.0.zip`。根据说明，instantclient-basic-nt-11.2.0.3.0.zip 包含了
一些 OCI、OCCI 和 JDBC-OCI 的基础性的必备的库，而 instantclient-sqlplus-nt-11.2.0.3.0.zip
包含了 SQL\*Plus 的相关库和可执行文件。

![软件下载页面](/resources/img/2013-07-16-zhi-zuo-oracle-sql-star-plusde-win32-portableban-ben/download_page.png)

注意，下载时，会要求 Oracle 的注册用户登录。没有的话，简单注册一个就可以了。

# 制作步骤2，解压合并软件包
将两个软件包解压到同一个目录，可以将这个目录命名为 `sqlplus-11.2.0.3.0-win32` 或者任何你喜欢的名字。这个目录的内容如下图所示：

![解压目录](/resources/img/2013-07-16-zhi-zuo-oracle-sql-star-plusde-win32-portableban-ben/unzip_dir.png)

这样，免安装版本制作完成。将这个目录拷贝到一个 Windows 32 位环境，即可直接使用。

# 如何连接到Oracle数据库

启动 CMD 命令行，导航到SQL\*Plus所在目录，使用如下命令即可连接到指定的 Oracle 数据库，
注意将{}之间的参数值替换你实际使用的、Oracle 数据库连接参数，保持命令及命令参数在同一行中

	D:\TMP\sqlplus-11.2.0.3.0-win32>sqlplus {USER}/{PWD}@(DESCRIPTION=(ADDRESS_LIST=(ADDRESS=(PROTOCOL=TCP)(HOST={HOST})(PORT={1521})))(CONNECT_DATA=(SERVICE_NAME={ORCL})))

![登录成功](/resources/img/2013-07-16-zhi-zuo-oracle-sql-star-plusde-win32-portableban-ben/logan.png)

- - -

# 修改历史
1.0.0，bigtree，2013年3月22日  
release

1.0.0，bigtree，2013年7月15日  
迁移到Octopress

1.0.0，bigtree，2015年5月25日  
迁移到 Jekyll，符合 GFM 的语法

