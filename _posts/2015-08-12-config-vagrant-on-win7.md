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

# 安装 Virgant

从 [Virgant](https://www.vagrantup.com/downloads.html) 下载，当前的最新版本是 1.7.4。
安装完成后需要重启计算机。

# 配置 Virgant

* 设置 VAGRANT_HOME

如果不设置这个变量的话，Vagrant 的工作目录缺省在 `C:\Users\YOUR_USER_NAME\.vagrant.d`，
后续会产生很多文件，导致 C: 盘比较大。
执行下面的命令来设置 VAGRANT_HOME 到特定的位置。

    SETX VAGRANT_HOME "E:\vagrant\.vagrant.d"
    
执行行这个命令后，需要重启控制台生效。    

# 配置并运行一个虚拟机

## 选择 Virgant 镜像

从 [http://www.vagrantbox.es](http://www.vagrantbox.es) 选择可用的 Virgant 镜像，
Virgant 镜像是别人已经安装配置好各种软件的虚拟机镜像，适用于各种各样的用途，
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

## 网络配置

Vagrant的网络有三种模式：

1. 较为常用是端口映射，就是将虚拟机中的端口映射到宿主机对应的端口直接使用 ，在 Vagrantfile 中配置：

    config.vm.network :forwarded_port, guest: 80, host: 8080

guest: 80 表示虚拟机中的 80 端口， host: 8080 表示映射到宿主机的 8080 端口。

2. 如果需要自己自由的访问虚拟机，但是别人不需要访问虚拟机，可以使用 private_network，
并为虚拟机设置 IP ，在 Vagrantfile 中配置：

    config.vm.network :private_network, ip: "192.168.1.104"
    
192.168.1.104 表示虚拟机的 IP，多台虚拟机的话需要互相访问的话，设置在相同网段即可

3. 如果需要将虚拟机作为当前局域网中的一台计算机，由局域网进行 DHCP，那么在 Vagrantfile 中配置：

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

1. 先启动虚拟机，执行命令，

    vagrant up
    
启动完成后，在宿主机中运行 VirtualBox.exe，在 VirtualBox 管理器中看到正在运行的虚拟机。
给这个虚拟机设置虚拟光盘为 VBoxGuestAdditions.iso 文件。VBoxGuestAdditions.iso 文件
在 VirtualBox 的安装目录下。

2. 使用 putty 进入虚拟机，IP 和端口为 127.0.0.1:2222，

    sudo mount /dev/cdrom /media/cdrom
    cd /media/cdrom
    sudo ./VBoxLinuxAdditions.run # 安装
    sudo umount /media/cdrom
    sudo eject /dev/cdrom

3. 重启虚拟机，执行命令，

    vagrant reload
    
4. 用 putty 再登录，检查目录映射是否生效。

### 映射目录中文件的改变，无法自动侦测到

当在宿主机中修改映射目录中的文件时，如果虚拟机中某个程序正在运行，侦测这些文件是否有改变，
则这个程序可能不能正常工作，侦测不到文件变化了。我在虚拟机中使用 `jekyll s --host 0.0.0.0 --port 8080`
命令启动 Jekyll，但我在宿主机中修改了映射目录中的文章时，发现 Jekyll 没有侦测到文件变化了。
这个的确比较麻烦，导致修改文章后，必须手工重启 Jekyll。

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
     sudo apt-get install emacs24
     # sudo apt-get install emacs24 --only-upgrade
     sudo apt-get install git
     sudo apt-get install subversion
     
    
# Rials 环境配置
     
## 安装 Node.js

一些 Gem 需要一个 JavaScript 运行环境，安装 Node.js 作为系统的 JS 运行环境， 先安装 nvm，

    curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.25.4/install.sh | bash

将 .bashrc 中的 nvm 程序初始化片断剪切到 .bash_profile 的最后，再重新登录。

    export NVM_DIR="/home/vagrant/.nvm"
    [ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"  # This loads nvm


使用 nvm 安装 Node.js

    nvm ls # 查看已安装，类似于 rbenv versions
    nvm ls-remote # 查看可安装版本，类似于 rbenv install -l
    nvm install 0.12.7
    nvm use 0.12.7
    nvm current
    nvm alias default 0.12.7
     
## 安装 rbenv

    git clone https://github.com/sstephenson/rbenv.git ~/.rbenv
    echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile
    echo 'eval "$(rbenv init -)"' >> ~/.bash_profile
    sudo reboot 
    type rbenv # 验证安装是否成功
    
## 安装 ruby-build

    sudo apt-get install -y libssl-dev libreadline-dev zlib1g-dev
    git clone https://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
    rbenv install -l # 查看可用的版本
    rbenv install 2.2.2 # 安装最新的 2.2.2 版本
  
## 安装必备 Gems

    gem sources -l # 查看当前的源
    gem sources -r https://rubygems.org/
    gem sources -a https://ruby.taobao.org/
    gem update --system
    gem install bundle
    rbenv rehash
    
# 将当前虚拟机打包为 box


    
     
   
