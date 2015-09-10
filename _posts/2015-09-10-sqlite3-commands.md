---
layout: post
title: "SQLite 3 Client 命令说明"
date: 2015-09-10 15:31:00 +0800
comments: true
sharing: false
categories: SQLite SQLite3
---
# 安装(Ubuntu 14.04)

    sudo apt-get install -y sqlite3 libsqlite3-dev

# 命令行

命令格式为，

    sqlite3 [OPTIONS] FILENAME [SQL]

如果指定的数据库文件不存在，则会新创建一个指定名称的数据库文件。
这里的 [SQL]，不仅可以为 SQL 语句，也可以为内置命令。

* 帮助

     --help

* 显示版本

    --version

## 导出数据库结构及表的综合命令

    sqlite3 development.sqlite3 '.dump' > dump.sql

## 执行 SQL 语句

    sqlite3 development.sqlite3 'SELECT COUNT(*) FROM groups'


# 内置命令

这些命令均以一个点号(.)开头。在 SQLite3 客户端提示符下执行时，不需要分号(;) 结尾，
但在提示符下执行 SQL 语句时，SQL 语句要以分号结尾。

* .help

* .exit

* .quit

* .show

显示当前的各个设置项。

* .databases

显示当前客户端关联的 SQLite3 数据库名称及文件路径。

* .open ?FILENAME?

关闭当前数据库文件，并打开新的数据库文件。

* .tables ?TABLE?

显示数据库中的表，如果不指定匹配模式，则显示所有表；指定，显示与模式相匹配的表。
例如 .tables %g% 会显示所有名称中包含 g 的表。

* .schema ?TABLE?

显示表名与匹配模式相匹配的、表的创建语句，即显示创建表的结构的 SQL 语句。
如果没有相应的表名匹配到这个匹配模式，则什么也不显示。

* .indices ?TABLE?

显示表名与匹配模式相匹配的、表的索引。未指定后面的参数，显示数据库中所有的索引；
否则只显示表名与匹配模式相匹配的、表的索引。

* .read FILENAME

读取指定文件中的 SQL 执行。

* .dump ?TABLE?

以 SQL 语句的形式，导出表结构及其数据。未指定后面的参数，导出整个数据库；
否则只导出表名与匹配模式相匹配的表结构和数据。


# SQL 函数

* CURRENT_TIMESTAMP

当前时间戳，与 MySQL 的 now() 类似，使用示例，

    SELECT CURRENT_TIMESTAMP;
