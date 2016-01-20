---
layout: post
title: "Elnode介绍"
date: 2016-01-20 23:59:59 +0800
comments: true
sharing: false
categories: emacs elnode
---
[Elnode](https://github.com/nicferrier/elnode) 是用Emacs Lisp编写的，基于事件IO的Web服务器。具有依赖小、轻便、与Emacs配合方便的特点。适合一些需要简单Web服务器的场景。

安装直接通过包安装。

使用命令 `M-x list-elnode-servers` 会打开一个临时buffer，查看当前正在运行的Elnode实例。在这个buffer中，可以按 k来关闭某个实例。

使用命令 `M-x elnode-make-webserver` 创建一个实例，需要指定目录和端口。

当你启动服务器后，会发觉在只能以127.0.0.1这个IP访问。可以在 `.emacs` 文件中设置变量 `elnode-init-host` 来设置 Elnode服务器的监听 IP。设置为 `0.0.0.0` 则会监听本机的所有IP(Windows下无效，只能指定具体的IP地址)。

    (setq elnode-init-host "0.0.0.0")
    
也可以使用命令 `C-u M-x elnode-make-webserver`，会提示需要指定目录、端口和IP。
