---
layout: post
title: "Redis 基础"
date: 2015-10-20 18:31:00 +0800
comments: true
sharing: false
categories: redis
---

# 介绍

# 安装

Redis 官方并不支持 Windows 平台，但 Microsoft 维护有一个 [Windows 分支](https://github.com/MSOpenTech/redis)，但相对于官方版本，代码会旧一些。

## Ubuntu 下安装

从 [这里](http://download.redis.io/releases/) 下载源码，当前的最新版本是 3.2.0。

解压，

    tar -xvf redis-3.2.0.tar.gz

编译测试安装，

    make
    make test
    sudo make install

# 配置

# 启动

    redis-server # 服务启动，缺省端口 6379
    redis-cli # 客户端启动

# 命令参考

    redis-server -h # 帮助
    redis-server -v # 查看版本
