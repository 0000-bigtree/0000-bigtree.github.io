---
layout: post
title: "SNMP and Ruby"
date: 2014-08-09 02:09:05 +0000
comments: true
sharing: false
categories: SNMP Ruby
---

Ruby 对 SNMP(Simple Network Management Protocol) 的支持主要有两个 gem，
分别是 [snmp gem](https://github.com/hallidave/ruby-snmp) 和
[net-snmp gem](https://github.com/mixtli/net-snmp)。

snmp gem 是纯 Ruby 实现，当前的最新版本是 1.2.0，2014年1月发布，但目前仅支持 SNMPv2c，
因为是纯 Ruby 实现，可以运行在任何 Ruby 虚拟机上，包括 MRI、JRuby、Rubinius 等。
这里有一篇简短说明文章 
[http://www.erickcantwell.com/2011/04/snmp-and-ruby/](http://www.erickcantwell.com/2011/04/snmp-and-ruby/)。

net-snmp gem 当前的最新版本是 0.2.5，2012年3月发布，是对 C 实现的，[net-snmp](http://www.net-snmp.org) 库的包装，
C 实现的 net-snmp 库是多数 Linux 发行版中缺省的 SNMP 内置库，
实现了最新的 SNMPv3 规范，功能完善。net-snmp gem 通过 [ffi gem](https://github.com/ffi/ffi) 来与 net-snmp C 实现库交互，
严格来说，在MIR、JRuby、Rubinius 上都可以正常运行，因为这些 Ruby 虚拟机上都实现了 ffi gem。

net-snmp C 实现库在各个操作系统(主要是 Windows、Linux、OSX)下都可以编译，相应的，
net-snmp gem 在这些操作上都可以正常工作(当然前提是这些
操作系统上有 Ruby 虚拟机实现，并且 ffi gem 工作正常)。

下面是使用 snmp gem 做 SNMP GET PDU 操作的例子，JRuby 1.7.13，WindowXP-x86 测试成功：

{% highlight ruby %}
require 'snmp' # 使用 snmp gem
host = ARGV[0] || '127.0.0.1'
manager = SNMP::Manager.new(:Host => host, :Community => 'pulibc', :Port => 161)
response = manager.get(["1.3.6.1.2.1.1.3.0"]) # 待读取的 OID
response.each_varbind { |vb| puts vb.to_s }
{% endhighlight %}

下面是使用 net-snmp gem 做 SNMP GET PDU 操作的例子，(JRuby 1.7.13，net-snmp-5.5.1-1.x86，WindowXP-x86) 测试失败，
简单看了下错误，其中一类错误是 C 库函数与 Ruby 映射有不匹配。但
在 Linux 环境下(MRI 2.1.2，net-snmp-5.5-49.el6_5.1.x86_64，CentOS6.5-x64) 测试成功：

{% highlight ruby %}
require 'net-snmp' # 使用 net-snmp gem
host = ARGV[0] || '127.0.0.1'
session = Net::SNMP::Session.open(:peername => host, :community => "public" )
begin
  pdu = session.get("1.3.6.1.2.1.1.3.0")
  puts pdu.varbinds.first.value
rescue Net::SNMP::Error => e
  puts e.message
end
session.close
{% endhighlight %}

- - -

修改历史
1.0.0，bigtree，2014年8月9日  
created  

1.0.0，bigtree，2014年8月9日  
release

1.0.0，bigtree，2015年5月26日  
迁移到 Jekyll，符合 GFM 的语法

