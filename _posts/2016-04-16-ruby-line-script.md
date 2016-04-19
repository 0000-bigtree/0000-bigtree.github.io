---
layout: post
title: "Ruby 单行脚本"
date: 2016-04-18 23:00:00 +0800
comments: true
sharing: false
categories: ruby
---
# -e
Ruby 单行脚本最关键是 -e 参数的支持，有了它，才可以将脚本内容由命令行传递到 ruby 解释器中执行。

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

# -p
-p 与 -n 的语义相同，并且在每次循环结束时，将 $_ 打印到标准输出。

例如，下面的单行脚本将文件的每行前面增加一个 # 号，并输出该行到标准输出，

    ruby -pe'$_ = "#" + $_' *.rb

# -i
前面的 -p 可以把处理的每行结果打印出来，并原文件并没有修改。其中一种方式是先使用输出重定向，再替换原文件，例如，

    ruby -pe'$_ = "#" + $_' xxx.rb > new.rb
    mv new.rb xxx.rb

-i 可以更方便的处理这种情况，-i 后添加一个扩展名，如 -ibak(注意两者之间没有空格)，脚本中输出到标准输出时，会自动重定向到原文件中，并且会将原文件自动添加指定的扩展名后，做备份。例如，下面的脚本去掉所有当前目录下的 *.rb 文件中的注释行，并且将原来的文件备份为 *.rbbak，

    ruby -ibak -ne'puts $_ unless $_ unless =~ /^#/' *.rb

把当前目录下所有 *.rb 文件中的行都注释掉，

    ruby -ibak -pe'$_ = "#" + $_' *.rb

给 xxx.rb 文件添加上行号，其中 $. 表示 Kernel.gets 当前正在处理文件的行号，与 $<.lineno 同义($< 表示当前正在处理的文件，$<.file 是当前处理文件的 IO 对象，$<.filename 是文件名称)，

    ruby -i.bak -pe'$_ = ($.).to_s + ": " + $_' xxx.rb

也可以写作，

    ruby -i.bak -pe'$_ = ($<.lineno).to_s + ": " + $_' xxx.rb

将 1 个文件以行为单位，分割成多个文件，新的文件名为该行的行号，

    ruby -ne 'open("#{$.}", "w") {|f| f.puts $_}' file
