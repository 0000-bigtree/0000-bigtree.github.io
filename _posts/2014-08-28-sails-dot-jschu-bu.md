---
layout: post
title: "Sails.js 初步"
date: 2014-08-28 02:13:09 +0000
comments: true
sharing: false
categories: JavaScript Node Node.js Sails Sails.js
---

[Sails.js](http://sailsjs.org/) 是一个类似 [Rails](http://rubyonrails.org/) 的 JavaScript Web 框架。

# 安装

## 安装 Node 及 npm

略

## 全局安装 Sails.js

    npm install -g sails
    
当前的最新版本是 0.10.4

# 开始使用

## 创建项目

    sails new everyday-english
    
会创建好一个模板项目。
    
## 启动运行

    sails lift
    
可以通过 <http://localhost:1337/> 来访问。

## 创建API模板

    sails generate api sentence

命令成功执行后，会自动生成控制器 `api/controllers/SentenceController.js` 和模型 `api/models/Sentence.js`，输出如下信息：

    info: Created a new controller ("sentence") at api/controllers/SentenceController.js!
    info: Created a new model ("Sentence") at api/models/Sentence.js!

    info: REST API generated @ http://localhost:1337/sentence
    info: and will be available the next time you run `sails lift`.    

查看 SentenceController.js 和 Sentence.js，可以看到他们并不实质的内容，只是有一些模板代码：

{% highlight javascript %}
/**
 * SentenceController
 *
 * @description :: Server-side logic for managing sentences
 * @help        :: See http://links.sailsjs.org/docs/controllers
 */

module.exports = {
	
};
{% endhighlight %}

{% highlight javascript %}
/**
* Sentence.js
*
* @description :: TODO: You might write a short summary of how this model works and what it represents here.
* @docs        :: http://sailsjs.org/#!documentation/models
*/

module.exports = {

  attributes: {

  }
};
{% endhighlight %}

## 配置 MySQL 数据库连接

暂时对生成的 SentenceController.js 和 Sentence.js 不做改变。先配置数据库连接。
打开 `config/connections.js` 文件，可以看到里面已经生成了一些模板代码，找到类似 `someMysqlServer` 的部分，修改为如下内容：

```
mysql: {  
adapter: 'sails-mysql',
host: 'localhost',
port: 3306,
user: 'user',
password: 'pwd',
database: 'db',
charset: 'utf8'    
},
```

注意安装 sails-mysql 并写入项目依赖

    npm install sails-mysql --save
 
## 完成模型定义

修改 Sentence.js 为如下内容：

{% highlight javascript %}
/**
* Sentence.js
*
* @description :: TODO: You might write a short summary of how this model works and what it represents here.
* @docs        :: http://sailsjs.org/#!documentation/models
*/

module.exports = {

  attributes: {
    english: {
      type: 'string'
    },

		chinese: {
			type: 'string'
		}
  }
};
{% endhighlight %}

## 配置 models.js

`config/models.js` 是配置模型相关信息的地方。修改为如下内容：

{% highlight javascript %}
module.exports.models = {
  connection: 'mysql',
  migrate: 'alter'
};
{% endhighlight %}

先在 MySQL 中创建 `everyday` 数据库，然后重启项目，sails.js 重启时会自动生成相关的表，指定这个行为的是选项 `migrate`。

- - -

# 修改历史
1.0.0，bigtree，2014年8月28日  
created  

1.0.0，bigtree，2014年10月24日  
release

1.0.0，bigtree，2015年5月26日  
迁移到 Jekyll，符合 GFM 的语法

