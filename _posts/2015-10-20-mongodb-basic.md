---
layout: post
title: "MongoDB 基础"
date: 2015-10-20 19:31:00 +0800
comments: true
sharing: false
categories: mongodb
---

# 介绍

# 安装

## Ubuntu 下安装

参考如下链接 [https://docs.mongodb.org/manual/tutorial/install-mongodb-on-ubuntu/](https://docs.mongodb.org/manual/tutorial/install-mongodb-on-ubuntu/)

添加软件源的 KEY，

    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 7F0CEB10

添加软件源，

    echo "deb http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.0.list

更新软件列表，

    sudo apt-get update

安装 MongoDB，

    sudo apt-get install -y mongodb-org

# 配置

# 启动


启动，

    sudo service mongod start

验证启动，可查看，

    less /var/log/mongodb/mongod.log

停止，

    sudo service mongod stop

重启，

    sudo service mongod restart
