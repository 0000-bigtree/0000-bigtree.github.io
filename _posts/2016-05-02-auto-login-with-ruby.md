---
layout: post
title: "使用 Mechanize 自动登录上网"
date: 2016-05-02 23:59:59 +0800
comments: true
sharing: false
categories: ruby mechanize
---

公司上网时需要认证，未认证通过就访问网页时，会转向到一个认证页面，用来输入你的用户名和密码。每天至少需要做 1 次这样的事，感觉浪费时间，于是简单了解了一下，可以使用 [Mechanize](https://github.com/sparklemotion/mechanize) 来自动输入用户名和密码。

首先，访问登录认证页面，填写用户名及密码，通过判断返回的页面中是否包含 *退出* 这样一个链接来判断是否登录成功。

```ruby
require 'mechanize'

agent = Mechanize.new{ |agent|
  agent.user_agent_alias = 'Mac Safari'
}

agent.get('LOGIN_URL') do |page|
  # pp page # 打印页面内容，可以查看页面的结构
  login_form = page.forms[0] # 从页面结构可以看到，页面只有 1 个 form，即登录 form
  login_form.account = 'ACCOUNT'
  login_form.password = 'PASSWORD'
  login_form.checkbox_with(:name => 'remember_me').check
  result_page = login_form.submit
  # pp result_page

  is_ok = result_page.links.find { |l| l.text == '退出' }
  puts(is_ok ? 'OK' : 'NOT OK')
end
```

Mechanize 的缺点是只能处理静态内容的网页，如果页面中的一些元素，如登录 form，是由客户端的 JavaScript 脚本生成的话，则 Mechanize 是无能为力的。这个时候，需要获取经由浏览器生成(已经执行了 JavaScript 脚本)的页面，才能得到实际展示的内容。这种情况下，使用 [PhantomJS](http://phantomjs.org/) 是一个选择，本质上，它就是一个浏览器，基于 WebKit 做了一些修改。
