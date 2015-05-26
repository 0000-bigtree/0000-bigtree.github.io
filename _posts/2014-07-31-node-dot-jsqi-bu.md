---
layout: post
title: "Node.js 起步"
date: 2014-07-31 03:07:59 +0000
comments: true
sharing: false
categories: Node JavaScript Node.js
---
Node.js 是一个 JavaScript 运行平台，提供了 JavaScript 执行引擎(可以简单理解为解释器，
Interpreter)和平台库，使 JavaScript 可以脱离浏览器环境运行。

JavaScript 在 Node.js 平台上运行后，就脱离了受限的浏览器沙箱，能够直接访问操作系统上的资源，调用操作系统提供的功能。

在 Node.js 的支撑下，JavaScript 的应用范围大大拓宽了，使得 JavaScript 能够开发像网络服务器、应用程序框架之类的应用。
JavaScipt on Node.js 具有与 Java(平台) 相当的能力。在本地模块 (Native Modules，用C/C++编写的Node.js模块) 的支持下，
JavaScript on Node.js 几乎没有任何限制。

Node.js 使用 Chrome's JavaScript runtime (即Chrome浏览器使用的V8) 为缺省的 JavaScript 引擎。V8 是 Google 开发的，
开源 JavaScript 引擎，由 C++ 实现，在多个平台有移值版本，性能也非常高。

由于 JavaScript 是一门语言，是与具体实现无关的，理论上来说，也可以运行在 Java 虚拟机之上，
只要在 Java 虚拟机上提供了 JavaScript 引擎，则利用 Node.js 开发的 JavaScript 代码，也可以不加修改
就可以部署到 JVM 上运行。目前已经有具体的项目，在 JVM 上支持部署 Node.js应 用，即 [Nodyn](http://nodyn.io)。

类似于 JavaScript 在浏览器中 API 和设计风格，Node.js 提供了事件驱动的、异步的 API。在面向网络的应用中，
可以大幅度的提高并发连接数，提高资源利用效率。这也是 Node.js 着重宣传的特性之一。但就其本质来说，
非阻塞的、事件驱动的网络框架，并不是什么新鲜的东西，也不是 Node.js 或 JavaScript 独有的特性，
它是在操作系统底层非阻塞的 SOCKET API 的支持下，一种网络应用的设计模式而已。其他语言也有实现类似异步的、基于事件
的网络框架，如 C++ 有 ACE，Java 有 Netty、MINA，Python 有 Twisted，Ruby 有 eventmachine等。这些类似框架依然可以
达到、甚至超出 Node.js 所宣称的性能。但是在 Node.js 中提供的异步的，基于事件的API，相较于其他语言的实现，
与 JavaScript 这门语言(匿名函数、闭包等等特性)结合比较好，显得更为亲和和自然。

JavaScript 是一门解释性的语言，相对于编译型的语言，理论上会带来一些性能损失。但它也具有解释型语言的灵活和方便。
可以很方便地运行、调试、部署。具有相当高的开发效率。

Node.js 也极大的扩展了 JavaScript 程序员的能力。使得他们的世界不再局限于浏览器，提高了生产力水平。

Node.js 目前依然在高速发展中，社区蓬勃而充满生机。相关的模块正在被不断的添加进来，生态圈非常繁荣。

# 1.安装配置

## Windows下安装配置 Node.js

在 Windows 下，希望配置一个可移动的 Node.js 环境，可以很方便地拷贝和配置，不要有太多的看不见操作。
如读写注册表或文件拷贝程序文件到系统目录之类的隐形依赖。

### 下载和安装

从 [http://nodejs.org/download](http://nodejs.org/download) 下载msi格式的安装包，当前的最新稳定版本是 0.10.29。
msi 安装包中已经包括了 npm 组件，如果直接下载使用 node.exe，需要手动安装npm，比较麻烦。

nmp 是 Node.js 的模块管理系统，类似于 Ruby 的Gem，Python 的 Pip，或者 Java 中的 Maven。
利用它可以将 Internet 上不计其数的 Node.js 模块安装到本地使用，还可以升级、卸载、查看依赖等等，
对开发和部署非常方便。

在安装界面中，将默认的安装路径按你的需要修改，其他选项一般不需要改动，保持不变。

默认情况下，安装程序会自动将安装路径添加到系统可执行文件搜索路径。

### 配置 Path 系统变量，添加 node.exe 所在目录到系统可执行文件搜索路径

上文所述，如果使用了 msi 安装包的话，默认安装选项会自动完成 `Path` 系统变量的修改，
但安装完成后，如果需要修改 Node.js 的安装路径，可以手动修改 `Path` 系统变量。

运行命令行，可以看到类似如下输出：

	C:\node -v
    v0.10.30

### 安装配置 npm

上文所述，如果使用了msi安装包的话，默认安装选项会自动安装好 npm，
并将 npm 所在目录加入到系统可执行文件搜索路径(实际上，npm 与 node.exe 位于同一目录中)。

npm全 局安装时(全局安装是指运行 `npm install` 命令时，加上 -g 或 --global 参数，
例如 `npm install grunt -g` 会将 grunt 做全局安装，如果运行 `npm install grunt` 是执行本地安装，
安装的模块会放到当前目录下的node_modules当中)，
会缺省安装模块到 `%USERPROFILE%\Application Data\npm\node_modules` (Windows XP) 或
`%AppData%\npm\node_modules` (Windows 7)。

运行命令行，查看 npm 版本

	C:\npm -v
    1.4.21

查看当前安装的全局模块，假定Node.js的安装路径为 `C:\coder\noderjs\`，当前用户为 `currentUser`

    C:\>npm list -g
    C:\Documents and Settings\currentUser\Application Data\npm
    └─┬ grunt@0.4.5
      ├── async@0.1.22
     ...

更多的npm配置，可以参考npm文档。全局的配置文件在 `C:\coder\nodejs\nodejs\node_modules\npm\npmrc`，
也可以创建用户配置文件 `C:\Documents and Settings\currentUser\.npmrc`。

## 在OS X下安装配置Node.js

略

## 在Linux下安装配置Node.js

略

# 2.Hello World

在命令中输入 `node` 就直接进到 Node.js的交互模式(按Ctrl+D退出)：

    C:\>node
    >

交互模式相当于 Python 的交互模式，或 Ruby 的 irb，
在这个环境下，可以交互式地执行代码，查看到结果。非常动态化。
这个功能是解释型语言一般都会提供的、实现 REPL (Read, Eval, Print, Loop) 的方便工具。

查看当前 node.exe 的进程ID：

    > process.pid
    2596
    >

Node.js 默认提供了一些全局对象 (Global Objects)，对一些功能进行了封装。其中 process 封装了当前进程和运行环境的一些功能。
更详细的信息参考可以 Node.js 的 API 文档 [http://nodejs.org/api/globals.html](http://nodejs.org/api/globals.html)。

查看全局 process 对象的方法和属性：

    > process
    { title: 'C:\\WINDOWS\\system32\\cmd.exe - node',
      version: 'v0.10.30',
         ...
         'Binding signal_wrap',
         'NativeModule string_decoder' ],
      versions:
       { http_parser: '1.0',
         node: '0.10.30',
         v8: '3.14.5.9',
         ares: '1.9.0-DEV',
         uv: '0.10.28',
         zlib: '1.2.3',
         modules: '11',
         openssl: '1.0.1h' },
      arch: 'ia32',
      platform: 'win32',
      argv: [ 'node' ],
      execArgv: [],
      ...

使用模块：

    > require('os')
    { endianness: [Function],
      hostname: [Function],
      loadavg: [Function],
      uptime: [Function],
      freemem: [Function],
      totalmem: [Function],
      cpus: [Function],
      ...
      EOL: '\r\n' }
    > os.uptime()
    11422.3489512
    > os.totalmem();
    2146934784
    >

注意，跟全局对象不一样，模块使用前需要执行 `require` 函数，而不像全局对象，在任意模代码中直接可以使用。

更通常的方法是将程序代码放入文件中：

{% highlight javascript %}
// output.js
os = require('os');
console.info('my pc\'s cpu model is ' + os.cpus()[0].model);
console.info('my pc\'s name is ' + os.hostname());
console.info('my Node.js version is ' + process.version);
// read file
console.info('my boot.ini content showing below: ');
fs = require('fs')
fs.readFile('C:\\boot.ini', 'utf8', function (err, data) {
  if (err) {
    return console.error(err);
  }
  console.log(data);
});
{% endhighlight %}

在命令行中执行，显示如下输入：

    C:\>node output.js
    my pc's cpu model is Pentium(R) Dual-Core  CPU      E5400  @ 2.70GHz
    my pc's name is hahaha
    my Node.js version is v0.10.30
    my boot.ini content showing below:
    [boot loader]
    timeout=3
    default=multi(0)disk(0)rdisk(0)partition(1)\WINDOWS
    [operating systems]
    multi(0)disk(0)rdisk(0)partition(1)\WINDOWS="Microsoft Windows XP Professional" /noexecute=optin /fastdetect /noguiboot

# 3.复杂一点的例子

在 [Node.js](http://nodejs.org) 的主页上，有一个较复杂一点的例子，实现了一个简单的HTTP 服务器，
服务器运行后，当访问端口 1337 时，简单输出一个字符串“Hello World”。

{% highlight javascript %}
// server.js
var http = require('http'); // 引入http模块
http.createServer(function (req, res) { // 收到请求后执行该函数
  res.writeHead(200, {'Content-Type': 'text/plain'});
  res.end('Hello World\n');
}).listen(1337, '127.0.0.1'); // 监听端口1337
console.log('Server running at http://127.0.0.1:1337/');
{% endhighlight %}

运行：

    C:\>node server.js
    Server running at http://127.0.0.1:1337/

在浏览器中输入 <http://localhost:1337>，就可以看到“Hello World”字符串在页面显示出来。

下面我来实现另一个例子，用一个客户端去访问前面所述的 HTTP 服务器，获取其输出。
Node.js 也提供了 Http Client 的封装，代码如下：

{% highlight javascript %}
// httpclient.js
var http = require('http');  
http.get({host: '127.0.0.1', port: 1337}, function(res) {  
    res.setEncoding('UTF-8');  
    res.on('data', function (data) {  // 收到数据后打印
        console.log('response data: ' + data);  
    });  
});
{% endhighlight %}

运行：

    C:\>node httpclient.js
    response data: Hello World

# 4. 相关资源

[Node.js主页](http://nodejs.org)

Node.js 主页，故事从这里开始。

[npm/Node.js模块](https://www.npmjs.org)

可以从这里查找到不计其数的模块。有数据库驱动器 (支持MySQL、PostgreSQL等等)，
有模板引擎 (如 Jade)，消息推送系统 (如 Socket.IO)，Web 框架 (如express)，应有尽有。
当前 npmjs 网站显示，其模块数为：`Total Packages: 86 387`。

[Grunt](http://gruntjs.com)

相当于 make，或 Java 中的ant，或 Ruby 中的 rake。一个用 JavaScript 重复造的轮子。

[express](http://expressjs.com)

较为流行的 Web 开发框架。

- - -

# 修改历史
1.0.0，bigtree，2014年7月31日  
created  

1.0.0，bigtree，2014年8月1日  
release

1.0.0，bigtree，2015年5月25日  
迁移到 Jekyll，符合 GFM 的语法

