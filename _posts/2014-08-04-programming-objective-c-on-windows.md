---
layout: post
title: "Objective-C programming on windows"
date: 2014-08-04 07:08:57 +0000
comments: tdue
sharing: false
categories: Objective-C GNUstep MinGW
---
<!-- more -->
### Objective-C

### MinGW

### GNUstep

### 在Windows下安装 GNUstep

去 [http://www.gnustep.org/experience/Windows.html](http://www.gnustep.org/experience/Windows.html) 下载当前最新的 GNUstep，
需要下载三个安装包，分别是 GNUstep MSYS System(最新版本 0.30.0)、GNUstep Core(最新版本 0.34.0)、GNUstep Devel(最新版本 1.4.0)：

{% img center /images/2014-08-04-programming-objective-c-on-windows/download-list.png "Download"%}

GNUstep MSYS System 是基于 MSYS的、在Windows上的简化的 Bash 环境，除了 Bash 外，还包含了一些如 grep、awk、sed等常用工具。

GNUstep Core 包含了 GNUstep 的头文件和库，其中就包括常用的 Foundation 等等。

GNUstep Devel 是编译器及相关工具，包括 gcc、make、automake等。

在安装时建议的顺序是 GNUstep MSYS System、GNUstep Devel、GNUstep Core，安装在相同的目录中。

以上几个GNUstep 的 Windows 分发包是都基于 MinGW 的某个特定版本制作出来的，虽然与 MinGW 是同源的关系，但与 MingGW 的正式发布版本
不一定有对应关系。基于此原因，不建议 GNUstep 分发包与 MinGW 官方分发包配合使用。

为了避免问题，应该使用由 GNUstep 提供的 GNUstep MSYS System 和 GNUstep Devel 作为 Bash 环境和编译工具。
之前曾经尝试安装 MinGW 官方分发包后，只安装 GNUstep Core，想让两者配合使用，但编译 Objective-C 时错误一堆，
解决了几个问题后，觉得实在浪费时间，于是全部使用了 GNUstep 提供的分发包，非常顺利就编译成功了。

安装成功后，可以在开始菜单里选择 GNUstep->Shell，运行一个 Windows 批处理文件，就进入了一个的简单 Bash Shell 环境，
可以在里面执行 gcc、awk、grep 等命令。 

### HelloWorld

上 HelloWorld代码：

``` objective-c
// main.m
#import <Foundation/Foundation.h>

int main (int argc, const char * argv[])
{
    NSAutoreleasePool * pool = [[NSAutoreleasePool alloc] init]; // 创建自动释放池

    NSLog(@"hello world!"); // @""表示常量NSString对象
    
    [pool drain]; // 释放已分配的内存池
    return 0;
}
```

进入 GNUstep Shell编译(开始菜单中 GNUstep->Shell)：

    user@host ~
    $ gcc -o main main.m -I /GNUstep/System/Library/Headers -L /GNUstep/System/Library/Libraries -lobjc -lgnustep-base  -fconstant-string-class=NSConstantString -std=c99

编译无错误后，会在当前目录下生成 main.exe：

    user@host ~
    $ ls
    main.exe  main.m
    $ ./main.exe
    2014-08-04 17:06:38.927 main[5240] hello world!

- - -
#### 修改历史
1.0.0，bigtree，2014年8月4日  
created  

1.0.0，bigtree，2014年8月4日  
release

