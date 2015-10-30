---
layout: post
title: "在 Rails 项目中使用预制数据"
date: 2015-10-29 18:00:00 +0800
comments: true
sharing: false
categories: rails seed db
---

在 Rails 应用(或其他应用)中，总有一些预制的数据，这些数据是在应用正式使用之前预制的。它们有可能是相关配置参数，用来控制应用的一些行为。也有可能是权限控制里的缺省用户。甚至是业务相关的一些初始数据等等。

应用中这些预制数据的存储，可以是任意形式或任意位置。但一般主要有两种，文件存储和数据库存储。Rails 应用中，文件存储一般采用 YAML 的格式，主要存储系统级的、精简的数据。而在数据库中一般存储数量大的、复杂的、业务相关的数据。数据库的预制数据可以采用 SQL 文件的方式，即把所有的数据以 SQL 语句的形式保存在一个或多个 .sql 文件中，再使用相关工具导入到数据库中，但这种方式比较繁杂，SQL 语句冗长且不好理解，另外，由于各个数据库有一些差别，导致这些 SQL 文件不一定能够通用。


# Rails 内置支持

Rails 内置了对预制数据的支持。可以把应用的内置数据放在 db/seeds.rb 中。这些数据是 Ruby 代码，通过 activerecord 相关的存储到数据库中。并且，这些代码不受限于某个特定的数据库类型。例如，

~~~
users = User.create([
                     {
                       email: 'test-user-00@mail.com',
                       name: 'test-user-00',
                       password: '123456',
                       activated_at: DateTime.now,
                       admin: false
                     },
                     {
                       email: 'test-user-01@mail.com',
                       name: 'test-user-01',
                       password: '123456',
                       activated_at: DateTime.now,
                       admin: false
                     }
                    ])

100.times.each do |index|
  contact = Contact.new
  contact.name = "#{index}Name"
  contact.email = "love_#{index}@hotmail.com"
  contact.comment = "this comment for #{index}Name"
  contact.save
end
~~~

以上面的代码中，seeds.rb 创建了 2 个 用户及 100 个联系人对象。

执行命令，

    rake db:seed

将会把 seeds.rb 中的对象持久化到数据库中。

# seed-fu Gem

[seed-fu](https://github.com/mbleigh/seed-fu) 是一个更加方便的 Gem，相对于 Rails 内置的机制，更加灵活和强大。seed-fu 约定各个模型的预制数据存储在 #{Rails.root}/db/fixtures 和 #{Rails.root}/db/fixtures/#{Rails.env} 中，文件名 model 的复数名称，小写。
例如，User 模型的预置数据文件是 users.rb。

执行命令，

    rake db:seed_fu

将数据导入到数据库中。

另外，seed-fu 还提供了很多强大的、企业级的功能，比如，可以先将大的预制rb文件，压缩为 gzip 文件，seed-fu 可以直接读取 gzip 文件；灵活的命令行支持，可以指定 FIXTURE_PATH、FILTER 变量等等。

其他类似的 Gem 有，[activerecord-import](https://github.com/zdennis/activerecord-import)，[seedbank](https://github.com/james2m/seedbank) 等。
