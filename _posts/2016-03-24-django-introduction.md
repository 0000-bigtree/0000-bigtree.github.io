---
layout: post
title:  "Django 初体验"
date:   2016-03-26 00:00:00
categories: python django
---

Django是最有名的Python Web框架之一，网上看了一下与Rails的对比，国内的内容，大部分都是倒向Rails的。虽然了解一些Rails，但并不了解Django，于是先学习一下，有个切身的了解。

这是一篇学习笔记，关于慕课网的 [django初体检](http://www.imooc.com/learn/458) 课程。针对自己的 OSX 环境，内容有一些小变动。

# 安装 Python

使用 [pyenv](https://github.com/yyuu/pyenv) 来管理 Python 的多个版本，pyenv 类似于 rbenv 或 rvm(实际上就是 rbenv 改的，把安装 ruby 变成了安装 python)，用来管理同一机器上的多个 Python 版本。

    brew install pyenv
        
由于 pyevn 是 rbenv 改的，所以命令用法是一样的，安装 Python 版本，

    pyenv install 3.5.1
    pyenv global 3.5.1 # 设置当前使用
    
# 创建虚拟环境

这个功能是通过 pyenv 的插件 [pyevn-virtualenv](https://github.com/yyuu/pyenv-virtualenv) 实现的，它跟 virtualenv 并不是同一个东西，尽管 virtualenv 也可以创建虚拟环境。

    pyenv virtualenv 3.5.1 django_first # 删除为 pyenv uninstall django_first
    pyenv activate django_first # 激活，去激活为 pyenv deactivate
    pip install ipython # 安装 ipython，ipython 是一个更好的 REPL，即更好的 Python 交互环境，让 django 的一些功能用起来更方便高效
    pyenv rehash

# 安装 Django

    pyenv activate django_first
    pip install django
    pyenv rehash
    django-admin --version # 查看安装的 django 的版本

# 创建 Django 项目

执行下面的命令，会创建项目目录(项目名称只允许数字、下划线及字母的组合)，

    django-admin startproject django_first

目录的结构如下，

    ├── django_first
    │   ├── django_first
    │   │   ├── __init__.py
    │   │   ├── settings.py
    │   │   ├── urls.py
    │   │   └── wsgi.py
    │   └── manage.py
    
执行如下命令运行开发服务器，通过 [http://127.0.0.1:8000](http://127.0.0.1:8000)，访问页面，

    cd django_first
    python manage.py runserver

![缺省页面](/resources/img/2016-03-24-django-introduction/first.png)

# 项目目录介绍

## manage.py

执行项目管理的命令，如运行服务器，装载数据，运行数据库脚本等。常用的命令有：

运行开发服务器

    python manage.py runserver 0.0.0.0:8080 # 指定 IP 及端口运行开发服务器
    python manage.py runserver # 不指定 IP，缺省为 127.0.0.1，端口 8000
    
执行 shell(类似 rails c)

    python manage.py shell # 如果像前面那样装了 ipython，会以它作为交互环境

## django_first 目录

就是在项目目录下，与项目名相同的一个子目录。主要是存放项目的一些基础设施文件。

## django_first/settings.py

项目配置文件。常用的配置项有，

ALLOWED_HOSTS，允许访问的客户端 IP。

INSTALLED_APPS，已安装的应用，这个应用是 Django 里的应用概念。Django 里面的应用概念，用来划分功能，类似于模块，是一种业务逻辑的组织方式，可以作为独立的组件，在其他 Django 项目中复用。

MIDDLEWARE_CLASSES，中间件，与 rack 里的中间件类似。

ROOT_URLCONF，指定 urls 根文件。

TEMPLATES，模板引擎配置。

DATABASES，数据库配置。
 
STATIC_URL，静态文件路径。 

## django_first/urls.py

URL 映射配置文件，指定访问一个 url 时，应该被哪个类的方法响应处理。类似于 rails 里的 routes.rb

## django_first/wsgi.py

WSGI(Web Server Gateway Interfac)，即 Django 项目与 Web 服务器的通信接口。类似 Ruby 中的 rack，把 Web 项目(在这里是 Django 项目)与 Web 服务器的调用标准化，包括调用内容及调用方式等。有了 WSGI 标准定义后，任何 Python 程序都可以与 Web 服务器交互，并且是通用的，以相同的方式。 wsgi.py 用来配置与应用服务器交互的相关参数。


# 创建 Django 应用

在项目目录下，使用如下命令创建一下应用，应用名称为 blog，

    python manage.py startapp blog

会创建 blog 目录，结构如下，

    blog
    ├── __init__.py
    ├── admin.py
    ├── apps.py
    ├── migrations
    │   └── __init__.py
    ├── models.py
    ├── tests.py
    └── views.py

## 将应用添加到项目中

应用需要添加项目中，才能使用，项目才会把这个应用作为项目的一部分，进行管理，编辑 `django_first/settings.py`，在 `INSTALLED_APPS` 添加如下内容，

    INSTALLED_APPS = [
        'django.contrib.admin',
        'django.contrib.auth',
        'django.contrib.contenttypes',
        'django.contrib.sessions',
        'django.contrib.messages',
        'django.contrib.staticfiles',
        'blog', # 新增加的应用，上面是 Django 自带的应用
    ]

# 应用目录介绍

## blog/views.py

用来生成页面，在项目的 urls.py 中配置，根据访问 URL 来执行其中的一个方法，生成响应，类似于 rails 中的 controller。

## blog/models.py

定义数据库表。Django 1.7 之后，添加了 migration，可以根据 models.py 来产生 migration。

## blog/templates

存放模版的目录。

## blog/admin.py

Django 自带了后台管理的应用(django.contrib.admin)，这个应用也需要对项目中的其他应用管理，admin.py 就是 django.contrib.admin 应用用来管理同项目中其他应用的辅助文件。

# 在应用中创建一个最简单的页面

在上面创建的 blog 应用中创建一个最简单的页面，首先编辑 `blog/views.py`，变成如下代码，

    from django.shortcuts import render
    from django.http import HttpResponse
    
    # Create your views here.
    
    def hello(request):
        return HttpResponse('<html><body>Hello World!</body></html>')

编辑项目中的 `django_first/urls.py`，添加 URL 映射，

    from django.conf.urls import url
    from django.contrib import admin
    
    urlpatterns = [
        url(r'^admin/', admin.site.urls),
        url(r'hello/', 'blog.views.hello'), # 新增的 blog 应用中的一个 URL 映射
    ]
    
重新启动项目，即可查看结果，[http://127.0.0.1:8000/hello/](http://127.0.0.1:8000/hello/)，

    python admin.py runserver

# 开发blog应用页面

前面尝试编写了最简单的一个页面，了解了 Django 处理请求的基本过程。下面开始编写稍微正式一点的页面。blog 应用主要包含两个页面。一个页面，显示 blog 列表，一个页面，显示某条 blog 的详细内容，在列表页面中点击某条 blog，就可以导航到其详细内容页面。
