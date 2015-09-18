---
layout: post
title: "在 Win7 下安装配置 Vagrant 作为 Rails 开发环境"
date: 2015-08-13 14:31:00 +0800
comments: true
sharing: false
categories: vagrant virtualbox rails ruby
---

Ruby 对 Windows 的支持不太好，主要是使用了 C/C++ 代码的 Gem 对 Windows 支持不好，
在 Windows 下安装这些 Gem 时，能在当前计算机上正确编译这些 C/C++ 代码是一件艰难的事。
所以在 Windows 下开发 Rails 是一件很折腾人的事。
但是如果工作环境限定了要使用 Windows，可以用虚拟机的方式。简单来说就是在 Windows 上安装
虚拟机软件，在虚拟机安装 Linux，使用这个 Linux 来作为 Rails 的开发环境。
而 Vagrant 是让使用这个虚拟机环境更加方便的工具。

# 安装 VirtualBox

从 [VirtualBox](https://www.virtualbox.org/wiki/Downloads) 下载，当前的最新版本是 5.0。
除了下载虚拟机外，还要下载对应虚拟机版本的扩展包(Extension Pack)。

安装好 VritualBox 后，在 VirtualBox 管理器中， 管理->全局设定->扩展，安装下载好的扩展包。

# 配置 VirtualBox

* 设置 VirtualBox 的默认虚拟机位置

管理->全局设定->常规，配置默认虚拟机位置为 `E:\virtualbox-vms`，不修改的话，默认会在 C: 盘下。

# 安装 Vagrant

从 [Vagrant](https://www.vagrantup.com/downloads.html) 下载，当前的最新版本是 1.7.4。
安装完成后需要重启计算机。

# 配置 Vagrant

* 设置 VAGRANT_HOME

如果不设置这个变量的话，Vagrant 的工作目录缺省在 `C:\Users\YOUR_USER_NAME\.vagrant.d`，
后续会产生很多文件，导致 C: 盘比较大。
执行下面的命令来设置 VAGRANT_HOME 到特定的位置。

    SETX VAGRANT_HOME "E:\vagrant\.vagrant.d"

执行行这个命令后，需要重启控制台生效。

# 配置并运行一个虚拟机

## 选择 Vagrant 镜像

从 [http://www.vagrantbox.es](http://www.vagrantbox.es) 选择可用的 Vagrant 镜像，
Vagrant 镜像是别人已经安装配置好各种软件的虚拟机镜像，适用于各种各样的用途，
由于想自己从头开始，来安装配置 Linux下的 Rails 开发环境，所以选择了一个纯净版本的 Ubuntu 版本。
可以从这里下载 [ubuntu-14.04-amd64.box](https://github.com/kraksoft/vagrant-box-ubuntu/releases/download/14.04/ubuntu-14.04-amd64.box)。
将下载好的文件放入目录 `E:/vagrant/boxes`。

## 添加一个虚拟机模板

    cd E:/vagrant/boxes
    vagrant box add base ubuntu-14.04-amd64.box

上面的命令中，
base 表示指定默认的 box，也可以为 box 指定名称，如 rails，使用 base 这个名称时，
后面可以直接使用 vagrant init 进行初始化，如果自定义名称，初始化的时候需要指定 box 的名称。
ubuntu-14.04-amd64.box 是 box 对应的文件名，这里可以是本地保存 box 的路径，
也可以是可以下载 box 的网址，如果是网址的话，Vagrant 会自动启动下载。

命令输出如下：

    ==> box: Box file was not detected as metadata. Adding it directly...
    ==> box: Adding box 'base' (v0) for provider:
        box: Unpacking necessary files from: file://E:/vagrant/boxes/ubuntu-14.04-amd64.box
        box: Progress: 100% (Rate: 50.2M/s, Estimated time remaining: --:--:--)
    ==> box: Successfully added box 'base' (v0) for 'virtualbox'!

box 并不是真正后面要运行的虚拟机的文件，可以认为是一个虚拟机模板。后面创建的虚拟机，
会拷贝这个 box 作为基础，box 里面的系统已经安装了一些软件，以这个 box 为基础来创建虚拟机，
会大大节省安装配置这些软件的时间。
Vagrant 也提供了将一个虚拟机打包为 box 的方法，
就是 `vagrant package --output NAME --vagrantfile FILE` 命令。

## 初始化虚拟机

先选定一个虚拟机目录，在这个目录中，会存放这个虚拟机的 Vagrant 配置文件，
后面启动虚拟机时，需要先切换到这个下来。

    CD /D E:\vagrant\work\rails
    vagrant init

从上面的说明可知，`vagrant init` 实际等同于 `vagrant init base`，直接 `vagrant init` 而不指定名称，其名称就是
base。`vagrant init` 执行后，会在当前目录下生成一个 Vagrantfile，这个文件实际上是一个 Ruby 源代码文件。读取其中
的配置，实际上就是执行它。

## CPU及内存配置

编辑 Vagrantfile 文件，

    config.vm.provider "virtualbox" do |vb|
      # Display the VirtualBox GUI when booting the machine
      vb.gui = false
      vb.cpus = 2 # Customize the amount of cpu on the VM:
      # Customize the amount of memory on the VM:
      vb.memory = "1024"
    end

## 网络配置

Vagrant的网络有三种模式：

(1). 较为常用是端口映射，就是将虚拟机中的端口映射到宿主机对应的端口直接使用 ，在 Vagrantfile 中配置，

    config.vm.network :forwarded_port, guest: 80, host: 8080

guest: 80 表示虚拟机中的 80 端口， host: 8080 表示映射到宿主机的 8080 端口。

(2). 如果需要自己自由的访问虚拟机，但是别人不需要访问虚拟机，可以使用 private_network，
并为虚拟机设置 IP ，在 Vagrantfile 中配置，

    config.vm.network :private_network, ip: "192.168.1.104"

192.168.1.104 表示虚拟机的 IP，多台虚拟机的话需要互相访问的话，设置在相同网段即可。

(3). 如果需要将虚拟机作为当前局域网中的一台计算机，由局域网进行 DHCP，那么在 Vagrantfile 中配置，

    config.vm.network :public_network

## 目录映射

一般情况下，进行开发时，源代码保存在宿主机中，方便地使用 IDE 等工具，
虚拟机环境一般只用来运行代码，所以这里就需要使用目录映射功能，将本地的目录映射到虚拟机的对应目录。

默认情况下，当前的工作目录，会被映射到虚拟机的 `/vagrant` 目录，
当前目录下的文件可以直接在 `/vagrant` 下进行访问，
当然也可以在通过 ln 创建软连接，如，

    ln -fs /vagrant/www /var/www

-f 表示 强制创建，如果目标文件已存在，会覆盖， -s 为软链接。
但是一般来说，不建议在虚拟机里面对外部的条件依赖过多，把虚拟机打包为 box 时，
有可能会导致以这个 box 为模板的、新建的虚拟机环境不能正常工作。

更推荐的方法是在 Vagrantfile 进行目录映射的操作：

    config.vm.synced_folder "D:/workspace", "/home/vagrant/workspace"

前面的参数 “D:/workspace” 表示的是本地的路径，
这里使用使用绝对路径，也可以使用相对路径，比如： “workspace”。
后面的参数 “/home/vagrant/workspace” 表示虚拟机中对应的映射目录，必须是一个绝对路径。

### 不能设置共享文件夹的问题的解决

在执行 vagrant up 命令后，输出日志，发现，

    ==> default: Mounting shared folders...
        default: /vagrant => E:/vagrant/work/rails
    Failed to mount folders in Linux guest. This is usually because
    the "vboxsf" file system is not available. Please verify that
    the guest additions are properly installed in the guest and

配置的共享目录，在进入 linux 虚拟机后，也不能发现，google 一番，发现是 VBoxGuestAdditions 没有
在虚拟机中安装。

解决办法：

(1). 先启动虚拟机，执行命令，

    vagrant up

启动完成后，在宿主机中运行 VirtualBox.exe，在 VirtualBox 管理器中看到正在运行的虚拟机。
给这个虚拟机设置虚拟光盘为 VBoxGuestAdditions.iso 文件。VBoxGuestAdditions.iso 文件
在 VirtualBox 的安装目录下。

(2). 使用 putty 进入虚拟机，IP 和端口为 127.0.0.1:2222，

    sudo mount /dev/cdrom /media/cdrom
    cd /media/cdrom
    sudo ./VBoxLinuxAdditions.run # 安装
    sudo umount /media/cdrom
    sudo eject /dev/cdrom

(3). 重启虚拟机，执行命令，

    vagrant reload

(4). 用 putty 再登录，检查目录映射是否生效。

### 映射目录中文件的改变，无法自动侦测到

当在宿主机中修改映射目录中的文件时，如果虚拟机中某个程序正在运行，侦测这些文件是否有改变，
则这个程序可能不能正常工作，侦测不到文件变化了。我在虚拟机中使用 `jekyll s --host 0.0.0.0 --port 8080`
命令启动 Jekyll，但我在宿主机中修改了映射目录中的文章时，发现 Jekyll 没有侦测到文件变化了。
这个的确比较麻烦，导致修改文章后，必须手工重启 Jekyll。

## 自动执行脚本

Vagrantfile 中可以配置自动运行脚本，在虚拟机中的系统启动时执行，可以把命令直接写在
配置项里面，

    config.vm.provision "shell", inline: <<-SHELL
      echo 'abc' > ~vagrant/abc.txt
    SHELL

也可以指定文件，文件在宿主机中，

    config.vm.provision :shell, :path => "boot.sh"

boot.sh 内容，

    #!/usr/bin/env bash
    # PS1="${debian_chroot:+($debian_chroot)}\u@14.04:\w\$"
    echo 'abc' > ~vagrant/abc.txt

注意，只有使用 `vagrant up` 命令或 `vagrant reload` 时，只有虚拟机零下的第一次启动时，脚本才会执行；
后面执行，都需要加上参数 `--provision`，即 `vagrant up --provision` 和 `vagrant reload --provision`。
也可以立即执行，使用命令 `vagrant provision` 。

执行 `vagrant provision` 命令时，发现错误信息，

    ==> default: Running provisioner: shell...
        default: Running: inline script
    ==> default: stdin: is not a tty

不过不影响实际执行效果，脚本实际上是成功执行了的。如果有强迫症，网上有人提供了解决办法，
[http://foo-o-rama.com/vagrant--stdin-is-not-a-tty--fix.html](http://foo-o-rama.com/vagrant--stdin-is-not-a-tty--fix.html)，
添加一个 shell provision 先执行，

    config.vm.provision "fix-no-tty", type: "shell" do |s|
      s.privileged = false
      s.inline = "sudo sed -i '/tty/!s/mesg n/tty -s \\&\\& mesg n/' /root/.profile"
    end

实际上，putty 登录后，直接执行也可以，

    sudo sed -i '/tty/!s/mesg n/tty -s \\&\\& mesg n/' /root/.profile

放在 Vagrantfile 文件中，每次 shell provision 执行时，
都要执行一次，感觉直接执行命令更好，执行完一次就修复了，没有必要每次都执行。

## 配置 Linux

### 修改 Ubuntu 源

修改为网易源，网易开源镜像的网站是 [http://mirrors.163.com](http://mirrors.163.com)。

    cd ~
    cp /etc/apt/sources.list sources.list.bak
    wget http://mirrors.163.com/.help/sources.list.trusty
    mv sources.list.trusty sources.list
    sudo apt-get update


### 安装及更新软件

     sudo apt-get upgrade
     sudo apt-get install -y build-essential
     sudo apt-get install -y gdb
     sudo apt-get install -y cmake
     sudo apt-get install -y zip unzip
     sudo apt-get install -y rar unrar
     sudo apt-get install -y aspell
     sudo apt-get install -y vim --only-upgrade
     sudo apt-get install -y git tig
     sudo apt-get install -y subversion

设置时区，

    more /etc/timezone
    sudo dpkg-reconfigure tzdata


### 解决 .bashrc 没有自动执行问题

在 vagrant 的 HOME 目录下编辑 .bash_profile，在顶部添加

    [ -s ".bashrc" ] && . ".bashrc"

### 修改 SHELL 提示符

BASH 缺省的提示符是这样子的 `vagrant@vagrant-ubuntu-trusty:~/workspace`，
主机名太长了，直接把主机名固定为 `14.04`，即这个 ubuntu 系统的长期支持版本号。

编辑 .bashrc

    if [ "$color_prompt" = yes ]; then
        PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
    else
        PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w\$ '
    fi

把代表主机名的转义符 `\h` 变为 `14.04`，

    if [ "$color_prompt" = yes ]; then
        PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@14.04\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
    else
        PS1='${debian_chroot:+($debian_chroot)}\u@14.04:\w\$ '
    fi

### 安装配置 Emacs

    sudo apt-get install -y python-software-properties software-properties-common # 使命令 add-apt-repository 可用
    # sudo add-apt-repository ppa:cassou/emacs
    # sudo apt-get update
    # sudo apt-get install emacs24 emacs24-el emacs24-common-non-dfsg
    # sudo add-apt-repository --remove ppa:cassou/emacs
    # sudo apt-get update

找了几个源，都没有 Emacs 的最新版本，只能从源码来安装 Emacs，当前 Emacs 的最新版本是 24.5，

    cd ~
    wget http://mirrors.ustc.edu.cn/gnu/emacs/emacs-24.5.tar.gz
    cd emacs-24.5
    ./configure
    make
    sudo make install

使用 Emacs Preclude，

    cd ~
    git clone git://github.com/bbatsov/prelude.git ~/prelude
    rm -rf .emacs.d
    ln -s ~/prelude ~/.emacs.d

配置 .bashrc 或 .zshrc

    export TERM=xterm-256color


### 修改 SHELL 为 ZSH

    sudo apt-get install -y zsh
    sh -c "$(wget https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)" # 安装 Oh My ZSH

修改 .zshrc 文件，修改 theme，

    ZSH_THEME="robbyrussell"

变为，

    ZSH_THEME="ys"

修改命令行提示中的主机名，

    echo 'ubuntu-14.04' > .box-name

重启，

    vagrant reload


# Rials 环境配置

## 安装 JDK

JRuby 需要 Java 来支持，也可以只安装 JRE，但开发环境为了方便，直接安装 JDK。
当前的最新版本是 [JDK 8u60](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)。
下载压缩文件 jdk-8u60-linux-x64.tar.gz 后，解压到 ~/coder/java/jdk/jdk，然后在 .zshrc 中添加，

    export JAVA_HOME=/home/vagrant/coder/java/jdk/jdk
    export PATH=$JAVA_HOME/bin:$PATH

## 安装 Ant、Maven及Gradle

分别下载最新的 Ant 1.9.6、Maven 3.3.3 及 Gradle 2.6.0，解压后分别放到 ~/coder/java/ant/ant、
~/coder/java/maven/maven、~/coder/java/gradle/gradle。

    export ANT_HOME=/home/vagrant/coder/java/ant/ant
    export MAVEN_HOME=/home/vagrant/coder/java/maven/maven
    export GRADLE_HOME=/home/vagrant/coder/java/gradle/gradle
    export PATH=$ANT_HOME/bin:$MAVEN_HOME/bin:$GRADLE_HOME/bin:$PATH

## 安装 Node.js

一些 Gem 需要一个 JavaScript 运行环境，安装 Node.js 作为系统的 JS 运行环境， 先安装 nvm，

    curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.25.4/install.sh | bash

使用 nvm 安装 Node.js

    nvm ls # 查看已安装，类似于 rbenv versions
    nvm ls-remote # 查看可安装版本，类似于 rbenv install -l
    nvm install 0.12.7
    nvm use 0.12.7
    nvm current
    nvm alias default 0.12.7

安装常用的一些 module，

    npm install -g gulp
    npm install -g bower

## 安装 rbenv

    git clone https://github.com/sstephenson/rbenv.git ~/.rbenv
    echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile
    echo 'eval "$(rbenv init -)"' >> ~/.bash_profile
    sudo reboot
    type rbenv # 验证安装是否成功

## 安装 ruby-build

    sudo apt-get install -y libssl-dev libreadline-dev zlib1g-dev
    sudo apt-get install -y libmysqlclient-dev # for mysql2 gem
    sudo apt-get install -y sqlite3 libsqlite3-dev # for sqlite3 gem
    git clone https://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
    rbenv install -l # 查看可用的版本
    rbenv install 2.2.3 # 安装最新的 2.2.3 版本
    rbenv install jruby-1.7.22
    rbenv install jruby-9.0.0.0

## 安装必备 Gems

    gem sources -l # 查看当前的源
    gem sources -r https://rubygems.org/
    gem sources -a https://ruby.taobao.org/
    gem update --system -N
    gem install bundle -N
    rbenv rehash
    bundle config 'mirror.https://rubygems.org' 'https://ruby.taobao.org' # 配置 bundle gem 源镜像

# 使用 putty 自动登录

Windows 下，vagrant ssh 是没有办法使用的。登录虚拟机的控制台需要使用 putty，
缺省地址是 127.0.0.1:2222，但是每次都需要输入用户名密码(vagrant/vagrant)，比较麻烦。
有一篇文章讲述了自动登录的方法
[https://github.com/Varying-Vagrant-Vagrants/VVV/wiki/Connect-to-Your-Vagrant-Virtual-Machine-with-PuTTY](https://github.com/Varying-Vagrant-Vagrants/VVV/wiki/Connect-to-Your-Vagrant-Virtual-Machine-with-PuTTY)。

具体步骤是，

(1). 用 [puttygen](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)
转换 insecure_private_key 文件为 insecure_private_key.ppk 文件。

(2). 使用 putty 登录时，使用这个文件。

# 将当前虚拟机打包为 box

打包为 box 之后，其他地方可以很方便的复用这个虚拟机环境

    vagrant hault
    vagrant package --output ubuntu-14.04

--output 后面的参数 ubuntu-14.04 为 box 文件名，可自行指定。

# Vagrant 常用命令及注意点

* `vagrant up`，启动

* `vagrant halt`，停止

* `vagrant suspend`，暂时挂起，将虚拟机的状态保存在磁盘上，可以用 `vagrant up`
或 `vagrant resume`，重新启动起来，会比停止之后再启动速度快。

* `vagrant status`，查看虚拟机状态

* `vagrant destroy`，销毁虚拟机，一切都没有了，除了虚拟机模板，小心使用

* `vagrant package`，打包当前虚拟机为 box

* 在虚拟机中使用 `sudo reboot` 重启，目录映射不起作用，使用 `vagrant reload` 才行

# 参考链接

[Using Vagrant for Rails Development](https://gorails.com/guides/using-vagrant-for-rails-development)
