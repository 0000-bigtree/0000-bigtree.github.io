---
layout: post
title: "在 Windows 下安装 MySQL 5.7.11"
date: 2016-04-06 18:00:00 +0800
comments: true
sharing: false
categories: mysql windows
---
# 安装

去 [http://dev.mysql.com/downloads/mysql/](http://dev.mysql.com/downloads/mysql/) 下载，以 zip 格式打包的 MySQL，下载后文件为 mysql-5.7.11-winx64.zip。解压到你希望的目录，这个目录就是 MySQL 安装目录。

复制安装根目录下的 my-default.ini 为 my.ini，在其 [mysqld] 部分添加如下内容，

    [mysqld]
    character_set_server=utf8 # 设置数据库缺省字符集为 utf8

用管理员打开 cmd.exe，进入到安装目录，执行如下命令，

    mysqld --initialize --user=mysql --console
    
注意会输出一些信息，关注最后一行所生成的临时密码，后面要用到，

    ...
    2016-04-06T06:26:14.182102Z 1 [Note] A temporary password is generated for root@localhost: TKk51x#bOi0S

将 mysqld 安装为 Windows 服务，并使用刚才创建的 my.ini，

    mysqld --install mysql --defaults-file=E:\mysql-5.7.11\my.ini

启动服务，

    net start mysql
    
登录，使用刚才生成的临时密码，

    mysql -uroot -p

修改为新的密码，

    set password = password('newpassword');

允许 root 远程登录，

```sql
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'password' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

最后，把安装目录的 bin 加入到 Path 用户变量，即可正常操作。

# 命令参考

## 启动与停止

    net start mysql
    net stop mysql
    mysqladmin shutdown -uroot -ppwd

## 客户端连接

    mysql --default-character-set=utf8 -uuser -p -Pport -hip

## mysqld 操作

    mysqld –-console # 显示控制台输出
    mysqld --install # 安装为服务，缺省服务名为 mysql，服务自动启动
    mysqld --install-manual # 安装为服务，缺省服务名为 mysql，服务手动启动
    mysqld --install MySQL(serviceName) --defaults-file=C:\my-opts.ini
    mysqld --remove # 移除服务，服务名为 mysql

