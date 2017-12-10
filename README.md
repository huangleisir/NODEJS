# NODEJS
centos搭建nodejs开发环境
--------------
1.安装 Node.js 环境
Node.js 是运行在服务端的 JavaScript, 是基于 Chrome JavaScript V8 引擎建立的平台。
下载并安装 Node.js
下载最新的稳定版 v6.10.3 到本地
wget https://nodejs.org/dist/v6.10.3/node-v6.10.3-linux-x64.tar.xz
下载完成后, 将其解压
tar xvJf node-v6.10.3-linux-x64.tar.xz
将解压的 Node.js 目录移动到 /usr/local 目录下
mv node-v6.10.3-linux-x64 /usr/local/node-v6
配置 node 软链接到 /bin 目录
ln -s /usr/local/node-v6/bin/node /bin/node

2.配置和使用 npm
配置 npm
npm 是 Node.js 的包管理和分发工具。它可以让 Node.js 开发者能够更加轻松的共享代码和共用代码片段
下载 node 的压缩包中已经包含了 npm , 我们只需要将其软链接到 bin 目录下即可
ln -s /usr/local/node-v6/bin/npm /bin/npm

--
配置环境变量
将 /usr/local/node-v6/bin 目录添加到 $PATH 环境变量中可以方便地使用通过 npm 全局安装的第三方工具
echo 'export PATH=/usr/local/node-v6/bin:$PATH' >> /etc/profile
生效环境变量
source /etc/profile

使用 npm
通过 npm 安装进程管理模块 forever
npm install forever -g

使用node.js  
敲入node -v 命令，看看node 版本
* 编写并运行自己的第一个脚本  
vim hello.js
  编写脚本  console.log("hello node.js");
  保存并关闭
  命令行执行该脚本  node hello.js 
    如何以守护线程的形式，执行该脚本nohup 方式 怎么写这个执行脚本命令呢  这个问题提的有水平
nohup node hello.js > hello.log &

* 用node.js 创建自己的第一个网关应用
 vim  server.js
插入：
var http = require('http');

http.createServer(function (request, response) {

    // 发送 HTTP 头部 
    // HTTP 状态值: 200 : OK
    // 内容类型: text/plain
    response.writeHead(200, {'Content-Type': 'text/plain'});

    // 发送响应数据 "Hello World"
    response.end('Hello World\n');
}).listen(8888);

// 终端打印如下信息
console.log('Server running at http://127.0.0.1:8888/');
保存并关闭server.js 
执行  nohup node server.js > server.log &  
http://123.207.19.193:8888/    




