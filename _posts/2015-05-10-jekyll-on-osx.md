---
layout: post
title:  "OS X 下使用 Jekyll 来搭建 Blog"
date:   2015-05-10 00:00:00
categories: jekyll OSX
---

# Jekyll 介绍

# 安装

## 安装 rbenv
 
    brew install rbenv

## 用 rbenv 安装最新的 Ruby 2.2.2

    rbenv install 2.2.2

## 安装 jekyll gem

    gem install jekyll -N
    rbenv rehash


`-N` 表示安装 gem 的时候，不要生成 gem 文档。`rbenv rehash` 是安装了有可执行命令的 gem 之后，
生成这个可执行命令的一个包装命令。

# 创建站点

    jekyll new static-site
    
就会在目录 static-site 中，创建一个模板站点，目录结构如下：

    ├── _config.yml
    ├── _includes
    │   ├── footer.html
    │   ├── head.html
    │   └── header.html
    ├── _layouts
    │   ├── default.html
    │   ├── page.html
    │   └── post.html
    ├── _posts
    │   └── 2015-05-20-welcome-to-jekyll.markdown
    ├── _sass
    │   ├── _base.scss
    │   ├── _layout.scss
    │   └── _syntax-highlighting.scss
    ├── about.md
    ├── css
    │   └── main.scss
    ├── feed.xml
    └── index.html

_includes 和 _layouts 是页面布局文件和页面模板。

_posts 目录中保存文章，格式一般是 `年-月-日-文章标题`，
_posts 中的 2015-05-20-welcome-to-jekyll.markdown 是创建新站点模板时，jekyll 自动生成的一篇文章。

_sass 和 css 包含的是站点使用的样式文件。

index.html 是站点首页。

about.md 是首页中 about 链接目标的内容。
一般用来对博客作者做一些简单介绍。

feed.xml 是对博客的 RSS 输出内容。

_config.yml 是站点的配置文件。

# 本地运行站点

    cd static-site && jekyll serve
    
会在本地把站点运行起来，缺省的网址是 [http://127.0.0.1:4000](http://127.0.0.1:4000)，可以看到如下图的内容：

![首页](/resources/img/2015-05-10-jekyll-on-osx/index.png)

# 使用 Theme

Theme 简单来说，就是站点的皮肤，实际上，缺省的样式已经很不错了，简洁美观，但是不一定符合每个人的口味。
如果你是设计师，当然可以自己编辑 *.scss 文件、页面布局文件和页面模板来设计你独一无二的页面。如果你的懒
人，或者想参考别人的设计，这里有一个站点 [http://jekyllthemes.org](http://jekyllthemes.org)。
使用的时候，一般都是直接把一个 jekyll 网站模板给你，在里面就可以开始写文章，在本地运行，查看结果，
使用起来并不复杂。

我选择的模板是 [Lanyon](https://github.com/poole/lanyon)。

# 定制化

## 页头

## 关于页面


# 发布到 Github

# 与 Octopress 的比较

