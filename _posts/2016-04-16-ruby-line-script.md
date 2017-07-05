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

-i 可以更方便的处理这种情况，-i 后添加一个扩展名，如 -ibak(注意两者之间没有空格)，脚本中输出到标准输出时，会自动重定向到原文件中，并且会将原文件自动添加指定的扩展名后，做备份，如果 -i 后面不指定扩展名，则不会产生备份文件。例如，下面的脚本去掉所有当前目录下的 *.rb 文件中的注释行，并且将原来的文件备份为 *.rbbak，

    ruby -ibak -ne'puts $_ unless $_ unless =~ /^#/' *.rb

把当前目录下所有 *.rb 文件中的行都注释掉，

    ruby -ibak -pe'$_ = "#" + $_' *.rb

给 xxx.rb 文件添加上行号，其中 $. 表示 Kernel.gets 当前正在处理文件的行号，与 $<.lineno 同义($< 表示当前正在处理的文件，$<.file 是当前处理文件的 IO 对象，$<.filename 是文件名称)，

    ruby -i.bak -pe'$_ = ($.).to_s + ": " + $_' xxx.rb

也可以写作，

    ruby -i.bak -pe'$_ = ($<.lineno).to_s + ": " + $_' xxx.rb

将 1 个文件以行为单位，分割成多个文件，新的文件名为该行的行号，

    ruby -ne'open("#{$.}", "w") {|f| f.puts $_}' file-name

# -a
前面说过，-n 参数指定时，ruby 会自动把指定的文件逐行读入全局变量 $_ 中，而 -a 表示 ruby 会再对 $_ 内容做分割，分割到 $F 全局变量中。缺省情况下，以空格分割。

下面的代码实现，将 1 个文件以行为单位，分割成多个文件，新的文件名为该行的行号，新文件的内容，为该行内容以空格做分割后的数组，

    ruby -a -ne'open("#{$.}", "w") {|f| f.puts $F}' file-name

如果不用 -a 参数，也可以直接对 $_ 分割，下面的代码跟上面是等价的，

    ruby -ne'open("#{$.}", "w") {|f| f.puts $_.split}' file-name

# -F
-a 指定了自动分割 $_ 的行为，但缺省以空格分割，-F 指定分割字符。另外，在 ruby 中，$; 全局变量代表 String#split 方法缺省分割字符串的正则表达式，当$; 为 nil 时，以空格分割，可以用如下的代码来验证，

    ruby -e'puts $;.nil?, "a,b c".split'

    ruby -F, -e'puts $;.nil?, "a,b c".split'

综合例子，用逗号来分割原文件中的行(行中包含了以逗号分隔的多个字段)，调整字段次序后，再以 tab 分隔调整次序后的字段，写回原文件，

    ruby -a -F, -i -ne'puts $F.values_at(1, 0, 2, 3).join("\t")' file-name

# -0
指定记录分隔符(或者行分隔符)，使用 -n 读取文件时，默认的记录分隔符(行分隔符)就是操作系统的分隔符，但可以用 -0 来覆盖默认设置。使用时，-0 后面紧跟 8 进制字符串编码，-00 表示记录的分隔符是 2 个\n，即 2 个换行符，-0(即没有参数)或 -0777 表示把整个输入文件视为 1 个记录，-0157 表示以小写 o 作记录分隔符。

# -l
是否要单独处理记录结束符(行结束符)，缺省情况下，按记录分隔符读取记录时，会自动把记录分隔符也读入 $_ 中，作为记录的一部分。但指定 -l 参数时，读取 1 条记录，会把记录分隔符剔除掉，即 $_ 中不会包括记录分隔符。指定了 -l，再指定 -p 时，输出时，又会自动把记录分隔符加到记录的末尾。

下面的代码会自动的把换行符(8 进制 15)添加到每条处理过后的新记录之后，

    ruby -015 -l -pe'$_=$_.size' file-name

# -r
相当于在 -e 指定的代码之前，加了一句 `require library-name`。

    ruby -rdate -e 'puts DateTime.new'

等价的代码为，

```ruby
require 'date'
puts DateTime.new
```

启动内嵌的 webrick http 服务器，端口 5000，绑定到所有地址，以当前目录为工作目录，

    ruby -run -e httpd . -p 5000 -b 0.0.0.0

un 是一个包装了 webrick 功能的的模块，httpd 是其中的 1 个方法。

也可以直接使用 webrick(这段代码对于单行脚本来说，太长了),

    ruby -r webrick -e "s = WEBrick::HTTPServer.new(:Port => 9090, :DocumentRoot => Dir.pwd); trap('INT') { s.shutdown }; s.start"

使用 sinatra 的例子，

    ruby -rsinatra -e'set :public_folder, "."; set :port, 8000'

很多语言都有一行脚本运行 http 服务器，python 的例子如下，更多例子见 [https://gist.github.com/willurd/5720255](https://gist.github.com/willurd/5720255)。

    python -m http.server 8000 # python3
    python -m SimpleHTTPServer # python2

也可以自己编写库来运行，-I 表示在 $LOAD_PATH 中添加 1 个加载搜索路径，在 ruby 解释器的命令行中参数中，可使用多个 -I。

```ruby
# mini.rb
def hellp
  puts "Hello, #{ ARGV[0] }!"
end
```

    ruby -I . -rmini -e hello Jack

或

    ruby -r./mini -e hello jack

# 更多的例子

定时执行命令，

    ruby -e 'system "clear; ls -lahG; date" while sleep 1'

在当前目录及其所有子目录下，查找大小超过 1024 Bytes 的 Java 文件，并按最近修改时间排序

    ruby -e 'puts Dir["**/*.java"].find_all{|f| File.size(f) > 1024}.sort_by{|f| File.mtime(f)}'

# 参考

《Ruby 系统管理实战》，Andre Ben Hamou 著，仲田 等译
