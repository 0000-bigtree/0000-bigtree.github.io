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

# 开发 blog 应用页面

前面尝试编写了最简单的一个页面，了解了 Django 处理请求的基本过程。下面开始编写稍微正式一点的页面。blog 应用主要包含两个页面。一个页面，显示 blog 列表，一个页面，显示某条 blog 的详细内容，在列表页面中点击某条 blog，就可以导航到其详细内容页面。

从前面可知，在项目的 settings.py 已经安装了 Django 提供的 admin 等应用，先让它可以正常工作。

## 执行内置应用的迁移

    python manage.py migrate

## 创建超级用户

    python manage.py createsuperuser

执行了迁移之后，可以访问 [http://localhost:8000/admin](http://localhost:8000/admin)，以前面创建的超级用户登录，登录成功后可以看到如下的界面，

![admin页面](/resources/img/2016-03-24-django-introduction/admin-page.png)

## 设计model

即设计 blog 的数据模型。

打开 blog 应用目录下的 models.py，变成如下代码，

    from django.db import models
    from django.contrib import admin

    # Create your models here.
    class BlogsPost(models.Model):
        title = models.CharField(max_length = 150)
        body = models.TextField()
        timestamp = models.DateTimeField()

    admin.site.register(BlogsPost)

## 根据 model 创建数据迁移脚本

    python manage.py makemigrations blog
    python manage.py migrate # 执行迁移

再登录 admin 后台，可以看到已经有刚才创建的表，可以对这个表增删改查数据。

![blogpost页面](/resources/img/2016-03-24-django-introduction/admin-blogpost.png)

利用这个界面，创建几条 blog post 数据，以便后面使用。

因为 admin 应用中的 blogpost 列表只有一列，看 blogpost 数据不太方便，可以在 models.py 中创建一个 BlogPostAdmin 类，继承 admin.ModelAdmin父类，让 admin 应用来管理它，以表格的形式显示 blogpost 的标题和时间。

![blogpost list 页面](/resources/img/2016-03-24-django-introduction/admin-blogpost-list.png)

在 models.py 中添加，

    ...
    class BlogPostAdmin(admin.ModelAdmin):
        list_display = ('title','timestamp')

    admin.site.register(BlogsPost,BlogPostAdmin)

![blogpostadmin list 页面](/resources/img/2016-03-24-django-introduction/admin-blogpostadmin-lis.png)

## 开发 blog 主页面
### 创建模板

在 blog 应用的 templates 目录下创建 index.html 模板文件，内容如下：

    {% for post in posts %}
    <h2>{{ post.title }}</h2>
    <p>{{ post.timestamp }}</p>
    <p>{{ post.body }}</p>
    {% endfor%}

### 创建视图方法

修改 blog 应用的 views.py 为如下代码，

    from django.shortcuts import render_to_response
    from blog.models import BlogsPost

    def index(request):
        blog_list = BlogsPost.objects.all() # 获取数据库里面所拥有 BlogPost 对象
        return render_to_response('index.html',{'posts':blog_list})

### 添加 URL 模式

在项目的 urls.py 中添加 URL 映射如下，

    url(r'^blog/$', 'blog.views.index'),

访问 [http://localhost:8000/blog/](http://localhost:8000/blog/)，可以看到如下的页面，

![blog 主页面](/resources/img/2016-03-24-django-introduction/blog-index.png)

### 优化布局
### 使用父模板

这个类似 rails 里面的布局，在 blog 应用的 templates 里添加 base.html 模板，内容如下，

    <html>
        <style type="text/css">
         body{color:#efd;background:#453;padding:0 5em;margin:0}
         h1{padding:2em 1em;background:#675}
         h2{color:#bf8;border-top:1px dotted #fff;margin-top:2em}
         p{margin:1em 0}
        </style>

        <body>
            <h1>BLOG</h1>
            <h3>好好学习，天天向上</h3>
            {% block content %}
            {% endblock %}
        </body>
    </html>

修改 index.html 模板，让它引用 base.html 模板，并作为它的 content 块，

    {% extends "base.html" %}
    {% block content %}
    {% for post in posts %}
    <h2><a href="blog/{{post.id}}">{{  post.title }}</a></h2>
    <p>{{ post.timestamp | date:"1,F jS"}}</p>
    <p>{{ post.body }}</p>
    {% endfor %}
    {% endblock %}


### 项目主页

把项目的主页也定位到 /blog，可以在 项目的 urls.py 中添加，

    url(r'^$','blog.views.index'),

## 开发 blog 内容页面
### 模板

    <html>
        <body>
            <h2>{{ blog.title }}</a></h2>
            <p>{{ blog.timestamp | date:"1,F jS"}}</p>
            <p>{{ blog.body }}</p>
        </body>
    </html>

### 视图

    ...
    from django.template import loader
    ...

    def show(request, blogId):
        t = loader.get_template('blog.html')
        blog = BlogsPost.objects.get(id=blogId)
        context = {'blog': blog}
        html = t.render(context)
        return HttpResponse(html)

### URL


    url(r'^blog/(\d+)$', 'blog.views.show'),

# 参考链接

[http://my.oschina.net/matrixchan/blog/184445?fromerr=cqu8lHAp](http://my.oschina.net/matrixchan/blog/184445?fromerr=cqu8lHAp)
