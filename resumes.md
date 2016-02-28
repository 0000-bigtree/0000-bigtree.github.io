---
layout: page
title: 大树的简历
display-title: 大树的简历
---
# 请先看

感谢您浏览大树的简历，如果贵公司有以下情况之一，请忽略这份简历：

1. 考勤制度规定，正常情况下，每周工作天数多于 5 天。

2. 员工的保险、住房公积金缴纳不按实际工资，扣税按实际工资。

# 基本信息

姓名：&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;请见求职邮件  
网名：&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;大树  
性别：&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;男  
年龄：&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;36  
工作地：&nbsp;&nbsp;&nbsp;&nbsp;深圳  
最高学历：本科  
毕业时间：2003  
毕业学校：贵州财经学院  
专业：&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;计算机科学与技术  
学位：&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;工学学士  
Blog：&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<http://0000-bigtree.github.io>  
GitHub：&nbsp;&nbsp;&nbsp;<https://github.com/0000-bigtree>  
邮箱：&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;请见求职邮件  
手机：&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;请见求职邮件

# 技能

## 通用

+ 工作平台环境：Windows、Linux(Ubuntu、CentOS)、OSX 均无障碍。*nix环境中，可使用 zsh、bash。
+ VCS：svn、git
+ 数据库：MySQL、Oracle

## Java

+ 构建：Ant、Maven、Gradle
+ OSGi：Equinox、Felix
+ ORM：Hibernate、EclipseLink
+ Web：JSP/Servlet、Spring MVC、CXF、Wicket
+ Ioc：Spring
+ 网络：Mina、ActiveMQ
+ GUI：SWT、Swing、Eclipse RCP、Netbeans RCP

## Ruby

+ Ruby/JRuby
+ Web：Rails、Sinatra、Grape、kaminari、ancestry、simple_form、slim
+ Test：Rspec、factory_girl、faker
+ 构建：Rake
+ 网络：EventMachine、Celluloid-IO

## HTML/CSS/JavaScript

+ Bootstrap、JQuery、AngularJS、React

## 其他

### Node.js

+ Sails.js、Express、Gulp、Grunt、Webpack、NW.js、Electron

### Python

+ nosetests、requests

# 工作经历

##  深圳震有科技，网管研发，2010 年 1 月至 2016 年 1 月

工作介绍：

参与从零开始的、震有新一代网管产品研发，主要负责架构和核心业务实现。在服务端采用 OSGi 来实现面向 SOA 的业务设计，客户端使用 Netbeans 富客户端，以WebService(SOAP 风格)通信。适配公司自有设备(IP-PBX、光传输设备、基站等)接入网管系统，通过 SNMP、TR069、私有协议等适配公司 OEM 设备接入网管系统，实现设备统一、集中管理。系统中对脚本的支持使用 JRuby，以快速开发一些管理脚本，如设备数据的导入/导出、管理数据报表生成等。

还参与了公司其他产品的架构或开发工作，产品有：

1. 中国移动 MM(Mobile Market)服务身份标识服务，手机通过移动拨号上网时，获取 MM 客户端发出的 HTTP 请求，结合移动网络设备发送的手机号码信息，实现对原始 HTTP 请求的修改，再将请求其转发到后端服务，实现标识这个 HTTP 请求的用户(手机号码)的目的。 主要采用 Java 对网络 HTTP 协议解析，修改、转发，通过 RADIUS、UDP 与网络设备通信，获取手机号码等信息。

2. 长江航道局水文监测系统，主要是与航道内的水位终端、定位终端等设备通过运营商移动网络通信，解析协议，获取水文数据、GPS 数据等，传送到后端服务分析。通过 Java 使用 TCP 与终端通信。Web 展现用 OSGi、JSP/Servlet、Bootstrap、JRuby等。

3. 牛信服务，类似微信的企业级应用，作为一个集成 API 服务器，与即时通信服务器、GIS 服务器、文件管理服务器、电话交换通信设备、短信设备等交互，把这些服务或设备的功能以统一的 REST API 或 WebSocket API 的形式展现给牛信客户端和其他第三方服务使用。使用 CXF、Hibernate、JRuby 等。

总结：

在此期间技术的广度有所拓宽，Ruby/Rails 开始使用，对 Node.js、Groovy、Python 也有一些具体实践。由于跟设备相关的开发工作比较多，对网络协议相关知识的深度上有所增强。

另外，也参加一些技术方向管理的工作，产品证书申请等。这方面的一些感受是，产品目标定位不能脱离具体环境(目标客户、市场环境、领导期望值、公司战略方向、资源限制等)，否则会事倍功半。

##  深圳华为技术，中研网管平台，2007 年 11 月至 2009 年 12 月

工作介绍：

iMAP(集成管理应用平台)安全业务模块，前台开发工程师，iMAP 是华为网管产品的基础平台，其他产品线(无线、固网、光传输等)的网管都是基于该平台二次开发的，后台使用 C++，前台使用 Swing(Java GUI 库)，使用 CORBA 在前后台之间通信。主要包括拓扑、安全、故障、安全、性能等几个大的公共业务模块。

主要工作为：

1. 参与新需求方案形成文档。 

2. 具体业务需求代码开发实现。 

3. 业务模块 bug 修复，评审会评审。 

4. 回复二次开发咨询，协助或参加产品线二次开发。 

5. 现场问题支持及解决。

总结：

在此期间，工作职责涉及的具体技术无特别之处。主要是技术深度和个人工作能力得以加强(包括沟通、规范性、协同等等)。原因是组织架构复杂、开发人员多，流程比较长、规章制度多、岗位责任繁杂、业务复杂、质量要求较高、问题响应要求及时等。

开阔了眼界，知道了开发大型软件产品时，几千人要怎么协同开发，以及相关的组织架构、规章制度、工作流程、技术管理等等。

## 贵州大森天元信息技术，研发，2003 年 6 月至 2007 年 9 月

工作介绍：

公司主要做外包项目为主，第 1 年使用 Oracle Developer 开发工具，第 2 年转向 Java，主要使用 SWT、 Eclipse RCP、Spring、Hibernate、JSP/Servlet、Struts、JSF 等等框架或库。

在业务上，由于各个项目差别太大，基本上没有什么积累。积累的主要是相关的技术经验，前期参与公司的各个项目开发，实现客户的需求，垒代码是主要工作。后期参与公司的技术平台设计和开发工作，形成公司的快速项目模板，模板项目主要是包括了配置好的、选定框架(如 Spring、Hibernate、log4j 等等)，一些公共业务模块(人员管理，日志管理、安全管理等等)，构建脚本，另外还有模板代码等，从而快速生成新项目的基础设施，规范开发流程、提高开发效率。

总结：

学会了写代码，写代码的具体的一些概念、技巧和意识，基本来自这个时间段。

# 自评

对写代码有热情，能够从中获得乐趣和满足，有时会沉迷，导致不理性的、追求完美的趋向。不是敏捷型的人，属于苦思才会有所收获的普通型。工作认真，好相处，遵守规章制度。对灵活和变通这类技巧的天赋差一些。
