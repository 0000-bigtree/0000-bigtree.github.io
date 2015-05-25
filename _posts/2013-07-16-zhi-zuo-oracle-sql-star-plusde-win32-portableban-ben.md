---
layout: post
title: "制作Oracle SQL*Plus的Wndows 32位 Portable版本"
date: 2013-03-22 00:04
comments: true
sharing: false
categories: Database Oracle SQL*Plus
---
### Oracle SQL*Plus简介
SQL*Plus是Oracle数据库的客户端连接工具，操作界面非常简洁，没有复杂的图形用户界面，仅提供命令行类似的操作界面，但特性丰富且功能强大。
SQL\*Plus之于Oracle数据库，类似于MySQL客户端(mysql)之于MySQL数据库，当然，SQL\*Plus与mysql在功能上没有可比性，两者不是同一个级别的。在不同的平台下，
Oracle提供了Oracle数据库的可执行安装程序，SQL\*Plus作为其中的一个组件，可单独安装或与Oracle数据库一起安装，但安装过程复杂，耗时较长。

<!-- more -->

本文说明了制作一个Windows 32位操作系统环境下、免安装的、SQL\*Plus版本，使用的SQL\*Plus的版本号为11.2.0.3.0。这样，当在多台机器上需要使用SQL\*Plus时，
直接拷贝使用，免去安装过程的麻烦。在其他版本的Windows操作系统环境下，SQL\*Plus免安装版本的制作过程类似，不再赘述。另外，限于时间，
**本文测试(实验)的Window 32位操作系统环境为Windows XP SP3，没有在其他Window 32位操作系统环境下(如32位Windows Vista、32位Windows 7等)做测试，但一般情况下，在这些Windows 32位操作系统环境中，也是适用的。**

### 制作步骤1，去Oracle官方网站下载需要的软件
去网址[http://www.oracle.com/technetwork/topics/winsoft-085727.html](http://www.oracle.com/technetwork/topics/winsoft-085727.html)下载
如下图标识的两个软件包，分别为instantclient-basic-nt-11.2.0.3.0.zip和instantclient-sqlplus-nt-11.2.0.3.0.zip。根据说明，instantclient-basic-nt-11.2.0.3.0.zip包含了
一些OCI、OCCI和JDBC-OCI的基础性的必备的库，而instantclient-sqlplus-nt-11.2.0.3.0.zip包含了SQL\*Plus的相关库和可执行文件。

{% img center /images/2013-07-16-zhi-zuo-oracle-sql-star-plusde-win32-portableban-ben/download_page.png "软件下载页面"%}

注意，下载时，会要求Oracle的注册用户登录。没有的话，简单注册一个就可以了。

### 制作步骤2，解压合并软件包
将两个软件包解压到同一个目录，可以将这个目录命名为“sqlplus-11.2.0.3.0-win32”或者任何你喜欢的名字。这个目录的内容如下图所示：

{% img center /images/2013-07-16-zhi-zuo-oracle-sql-star-plusde-win32-portableban-ben/unzip_dir.png "登录成功" %}

这样，免安装版本制作完成。将这个目录拷贝到一个Windows 32位环境，即可直接使用。

### 如何连接到Oracle数据库
启动CMD命令行，导航到SQL\*Plus所在目录，使用如下命令即可连接到指定的Oracle数据库，注意将{}之间的参数值替换你实际使用的、Oracle数据库连接参数，保持命令及命令参数在同一行中

	D:\TMP\sqlplus-11.2.0.3.0-win32>sqlplus {USER}/{PWD}@(DESCRIPTION=(ADDRESS_LIST=(ADDRESS=(PROTOCOL=TCP)(HOST={HOST})(PORT={1521})))(CONNECT_DATA=(SERVICE_NAME={ORCL})))

{% img center /images/2013-07-16-zhi-zuo-oracle-sql-star-plusde-win32-portableban-ben/logan.png "登录成功" %}

- - -
#### 修改历史
1.0.0，bigtree，2013年3月22日  
release

1.0.1，bigtree，2013年7月15日  
迁移到Octopress

