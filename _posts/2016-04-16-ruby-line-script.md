---
layout: post
title: "Ruby 单行脚本"
date: 2016-04-18 23:00:00 +0800
comments: true
sharing: false
categories: ruby
---
# -e
Ruby 单行脚本最关键是 -e 参数的支持，有了它，才可以将脚本内容由命令行传传递到 ruby 解释器中执行。

    ruby -e'puts "hello ruby!"'

其中，-e 与其参数内容 `'puts "hello ruby!"'` 之间可以有空格，也可以没有空格，从紧凑来考虑，不留空格。

# -n
-n 表示在 -e 指定的代码外围，隐式地包上一层 `while gets ... end` 块。gets 的功能是逐行循环读取命令行中指定的每个文件，并把读取的内容放进全局变量 $_ 中。

实现类似 grep 功能的 ruby 单行脚本如下(在 2 个文件中找到匹配有 password 的行并打印出来)，
    
    ruby -ne'puts $_ if $_ =~ /password/' /var/log/my.log1 /var/log/my.log2
    
等价使用 grep 的命令如下，

    grep password /var/log/my.log1 /var/log/my.log2

等价的完整 ruby 代码如下(不使用 -ne 参数)，

```ruby
# file-grep.rb
ARGV.each do |file|
  File.open(file) do |io|
    while line = io.gets
      puts line if line =~ /password/
    end
  end
end
```

执行，

    ruby file-grep.rb /var/log/my.log1 /var/log/my.log2
