---
layout: post
title: "在 Ubuntu 下安装 Source Code Pro 字体"
date: 2016-01-11 23:59:59 +0800
comments: true
sharing: false
categories: linux ubuntu "Source Code Pro" font
---

[Source Code Pro](https://github.com/adobe-fonts/source-code-proo) 是 Adobe 公司发布的一款开源的等宽编程字体，它非常适合用于阅读代码，支持 Linux、Mac OS X 和 Windows 等操作系统，而且无论商业或个人都可以免费使用。这款字体和微软的 Consolas 一样均定位于编程字体。

安装，

    [ -d /usr/share/fonts/opentype ] || sudo mkdir /usr/share/fonts/opentype
    sudo git clone https://github.com/adobe-fonts/source-code-pro.git /usr/share/fonts/opentype/scp
    sudo fc-cache -f -v

查看，

     fc-list  | grep "Source Code Pro"
