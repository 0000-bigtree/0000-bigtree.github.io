---
LAYOUT: POST
TITLE: "GDT WITH ORG MODE"
DATE: 2015-11-30 23:00:00 +0800
COMMENTS: TRUE
SHARING: FALSE
CATEGORIES: EMACS ORG-MODE GDT
---

# 什么是GDT

# GDT WITH ORG MODE

## 配置 Agenda 文件

可以用如下代码，

    (setq org-agenda-files (list "~/.todos/work.org"
                                 "~/.todos/projects.org"
                                 "~/.todos/home.org"
                                 "~/Documents/todo/" ))

可以加入文件或目录。如果是目录，该目录下的所有 .org 文件都会被加入列表。还可以在编辑任务文件(.org文件)时随时使用C-c [/] 将文件加入/移出列表。

## 快捷键

* M-RET，新建事项
* C-c C-c，添加tags
* C-c C-t，切换任务状态
* S-LEFT/RIGHT，切换任务状态
* S-UP/DOWN，切换任务优先级
* C-c C-s，添加 schedule 时间
* C-c C-d，添加 deadline 时间
* C-c /， 只列出包含搜索结果的大纲，并高亮，支持多种搜索方式。该功能可以按照多种方式检索，其中针对任务有两种方式：TODO 和 TODO key words，分别实现高亮所有TODO和具有特定关键字的TODO。
