---
layout: post
title:  "Django初体验"
date:   2016-03-26 00:00:00
categories: python django
---

Django是最有名的Python Web框架之一，网上看了一下与Rails的对比，国内的内容，大部分都是倒向Rails的。虽然了解一些Rails，但并不了解Django，于是先学习一下，有个切身的了解。

这是一篇学习笔记，关于慕课网的 [django初体检](http://www.imooc.com/learn/458) 课程。针对自己的 OSX 环境，内容有一些小变动。

# 安装Python

使用 [pyenv](https://github.com/yyuu/pyenv) 来管理 Python 的多个版本，pyenv 类似于 rbenv 或 rvm(实际上就是 rbenv 改的，把安装 ruby 变成了安装 python)，用来管理同一机器上的多个 Python 版本。

    brew install pyenv
        
由于 pyevn 是 rbenv 改的，所以命令用法是一样的，安装 Python 版本，

    pyenv install 3.5.1
    pyenv global 3.5.1 # 设置当前使用
    
# 创建虚拟环境

这个功能是通过 pyenv 的插件 [pyevn-virtualenv](https://github.com/yyuu/pyenv-virtualenv) 实现的，它跟 virtualenv 并不是同一个东西，尽管 virtualenv 也可以创建虚拟环境。

    pyenv virtualenv 3.5.1 django-first # 删除为 pyenv uninstall django-first
    pyenv activate django-first # 激活，去激活为 pyenv deactivate

# 安装 Django

    pyenv activate django-first
    pip install django
    pyenv rehash
    django-admin --versiond # 查看安装的 django 的版本

