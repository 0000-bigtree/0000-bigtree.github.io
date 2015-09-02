---
layout: post
title: "配置 jEdit"
date: 2015-08-18 11:31:00 +0800
comments: true
sharing: false
categories: jEdit
---

# jEdit

[jEdit](http://www.jedit.org/) 是一个以 Java 编写的编辑器。以 [BeanShell](http://www.beanshell.org/) 作为扩展脚本语言。

# 下载及安装

[下载](http://www.jedit.org/index.php?page=download)，当前的最新稳定版为 4.2.0。下载 Java-based installer,
因这个安装包是一个 Java 打包程序，可以在 Windows、Linux 及 OS X 上直接使用。

    java -jar jedit5.2.0install.jar

选择安装目录为 `D:\workspace\coder\java\jEdit\jEdit-5.2.0`。

# 配置
                                        
## 目录结构

在目录 `D:\workspace\coder\java\jEdit` 下新建目录 `jEdit\bin`、`jEdit\home`、`jEdit\etc`。

## Windows 启动脚本

jEdit.cmd 的内容为，

    @SETLOCAL
    @SET JEDIT_VER=5.2.0
    @SET JEDIT_HOME=%JEDIT_HOME%
    @SET JAVA_HOME=%JAVA_HOME%
    @START %JAVA_HOME%\bin\javaw -Duser.home=%JEDIT_HOME%\home -Dsun.java2d.noddraw=true -Xms32M -Xms1024M -jar %JEDIT_HOME%\..\jEdit-%JEDIT_VER%\jedit.jar -settings=%JEDIT_HOME%\etc -background -reuseview %*
    @ENDLOCAL

jEdit.vbs 的内容为，

    args = ""
    length = Wscript.Arguments.Count
    If length > 0 Then
    	For i = 0 To  length - 1
    		args = args + " " + """" + Wscript.Arguments(i) + """"
    	Next
    End If
    createObject("wscript.shell").run "jEdit.cmd" + args, 0

为了集成到右键菜单，(1) 修改注册表，新建项 `HKEY_CLASSES_ROOT\*\shell\jEdit\command`。
(2) 在这个项下，新建可扩充字符串值，其值为 `wscript %JEDIT_HOME%\bin\jEdit.vbs "%1"`，取名为 `new`。
(3) 导出项 `HKEY_CLASSES_ROOT\*\shell\jEdit\command`，保存为 `setting_right_menu.reg`。
(4) 用记事本打开 `setting_right_menu.reg`，
将内容 `"new"=` 修改为 `@=`，保存。(5) 导入刚才修改后的注册表文件 `setting_right_menu.reg`。
(6) 重新刷新注册表项 `HKEY_CLASSES_ROOT\*\shell\jEdit\command`，可以发现，项下有两个串值，
名称为 `(缺省)` 的字符串值，类型已经变化为 `REG_EXPAND_SZ`，
内容跟刚才新建的、名称为  `new` 的串值相同。(7) 名称为 `new` 的串值已经不再需要，删除它。

执行命令，设置用户变量 `JEDIT_HOME` 的值，
                                        
    SETX JEDIT_HOME D:\workspace\coder\java\jEdit\jEdit

修改用户的 `Path` 变量，附加 `%JEDIT_HOME%\bin;`。

## OS X 启动脚本

## 全局配置

### 普通选项

* 本地化->使用默认本地化：去勾选；本地化->语言：English。
* if open files are changed on disk: prompt
* Check for file change upon: view focus or visiting the buffer

### Appearence

* Button, menu and label font: Dialog 16 Plain
* Startup options->Show tips on startup, uncheck
* Experimental options->Use jEdit text area colors in all text components: check
* Draw dialog box borders using Swing look & feel: check

### Editing 

* Separate "CamelCased" words: check
* Tab width: 2
* Indent width: 2
* Soft(emulated with spaces): check
* What should I do about very big files: Ask

### Encodings

* Default character encoding: UTF-8
* Selected encoding(s): UTF-8, GB2312, GBK, GB18030, Big5, ISO-8859-1, US-ASCII

### Gutter

* Minimal number of digits to reverse for line numbers: 4
* Selection area witdh(in pixels): 16

### Mouse

* Drag and drop in text area: uncheck
* Double-click drag joints non-alphanumeric characters: uncheck

### Plugin Manager

* Preferred download mirror: Asia: Japan Advanced Institute of Science and Technology(Nomi, Japan)
* Install plugins in: jEdit settings directory

### Saving & Backup

* Backup directory: ~/backup

### Shortcuts

* 选择 keymap 为 Emacs，然后 duplicate，名称为 myEmacs。

### View

* Show buffer switcher: uncheck
* Sort buffer sets: uncheck
* Show the menu bar in full-screen mode: uncheck

### Text Area

* Text font: Monospaced 20 Plain
* Fractional font metrics: check

### File System Browser -> General

* Show hidden files: check

## 插件

移除 QuickNotepad

### Ancestor

### BufferTabs

* Close tab on->single middle click: uncheck
* Layout and Style Options->Do not stretch tabs to fill rows: check

### DirtyButter

### FTP

### Navigator

* Navigator->Use Navigator's 'go to line' Dialog: check

### ColumnRuler

* Ruler->General->Active by Default: check
* Show Navigator on toolbar 

### JavaSideKick

### Sessions

### Hex

### SshConsole

### Highlight

### JDiffPlugin


# 将配置纳入 git 版本控制

主要是将 JEDIT_HOME 环境变量指向的目录纳入管理，方便共享配置、版本升级和追踪配置变更。

.gitignore 的内容如下，

    etc/jars-cache
    etc/macros # 宏不放在该目录，而是放在其他地方，链接到该目录下
    etc/PluginManager.download
    etc/plugins
    etc/settings-backup
    etc/activity.log
    etc/history
    etc/killring.xml
    etc/mirrorList.xml
    etc/perspective.xml
    etc/pluginMgr-Cached.xml.gz
    etc/printspec
    etc/recent.xml
    etc/server
    etc/cache
    
    home
