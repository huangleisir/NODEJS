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

http://www.runoob.com/nodejs  这个里面内容很不错了  讲的很清楚
node.js 回调函数 这个非常重要 
阻塞代码 实现文件读取
-----------------------------------------------
Node.js 回调函数
Node.js 异步编程的直接体现就是回调。
异步编程依托于回调来实现，但不能说使用了回调后程序就异步化了。
回调函数在完成任务后就会被调用，Node 使用了大量的回调函数，Node 所有 API 都支持回调函数。
例如，我们可以一边读取文件，一边执行其他命令，在文件读取完成后，我们将文件内容作为回调函数的参数返回。这样在执行代码时就没有阻塞或等待文件 I/O 操作。这就大大提高了 Node.js 的性能，可以处理大量的并发请求。
阻塞代码实例
创建一个文件 input.txt ，内容如下：
菜鸟教程官网地址：www.runoob.com
创建 main.js 文件, 代码如下：
var fs = require("fs");

var data = fs.readFileSync('input.txt');

console.log(data.toString());
console.log("程序执行结束!");
以上代码执行结果如下：
$ node main.js
菜鸟教程官网地址：www.runoob.com

程序执行结束!
非阻塞代码实例
创建一个文件 input.txt ，内容如下：
菜鸟教程官网地址：www.runoob.com
创建 main.js 文件, 代码如下：
var fs = require("fs");

fs.readFile('input.txt', function (err, data) {
    if (err) return console.error(err);
    console.log(data.toString());
});

console.log("程序执行结束!");
以上代码执行结果如下：
$ node main.js
程序执行结束!
菜鸟教程官网地址：www.runoob.com
以上两个实例我们了解了阻塞与非阻塞调用的不同。第一个实例在文件读取完后才执行完程序。 第二个实例我们不需要等待文件读取完，这样就可以在读取文件时同时执行接下来的代码，大大提高了程序的性能。
因此，阻塞是按顺序执行的，而非阻塞是不需要按顺序的，所以如果需要处理回调函数的参数，我们就需要写在回调函数内。
-----------------------------------------------------------
Node.js 事件循环
# Node.js 是单进程单线程应用程序，但是通过事件和回调支持并发，所以性能非常高。
Node.js 的每一个 API 都是异步的，并作为一个独立线程运行，使用异步函数调用，并处理并发。
Node.js 基本上所有的事件机制都是用设计模式中观察者模式实现。
Node.js 单线程类似进入一个while(true)的事件循环，直到没有事件观察者退出，每个异步事件都生成一个事件观察者，如果有事件发生就调用该回调函数.
事件驱动程序
Node.js 使用事件驱动模型，当web server接收到请求，就把它关闭然后进行处理，然后去服务下一个web请求。
当这个请求完成，它被放回处理队列，当到达队列开头，这个结果被返回给用户。
这个模型非常高效可扩展性非常强，因为webserver一直接受请求而不等待任何读写操作。（这也被称之为非阻塞式IO或者事件驱动IO）
在事件驱动模型中，会生成一个主循环来监听事件，当检测到事件时触发回调函数。
![111](http://www.runoob.com/wp-content/uploads/2015/09/event_loop.jpg)

整个事件驱动的流程就是这么实现的，非常简洁。有点类似于观察者模式，事件相当于一个主题(Subject)，而所有注册到这个事件上的处理函数相当于观察者(Observer)。
Node.js 有多个内置的事件，我们可以通过引入 events 模块，并通过实例化 EventEmitter 类来绑定和监听事件，如下实例：
// 引入 events 模块
var events = require('events');
// 创建 eventEmitter 对象
var eventEmitter = new events.EventEmitter();
以下程序绑定事件处理程序：
// 绑定事件及事件的处理程序
eventEmitter.on('eventName', eventHandler);
我们可以通过程序触发事件：
// 触发事件
eventEmitter.emit('eventName');
实例
创建 main.js 文件，代码如下所示：
// 引入 events 模块
var events = require('events');
// 创建 eventEmitter 对象
var eventEmitter = new events.EventEmitter();

// 创建事件处理程序
var connectHandler = function connected() {
   console.log('连接成功。');
  
   // 触发 data_received 事件 
   eventEmitter.emit('data_received');
}

// 绑定 connection 事件处理程序
eventEmitter.on('connection', connectHandler);
 
// 使用匿名函数绑定 data_received 事件
eventEmitter.on('data_received', function(){
   console.log('数据接收成功。');
});

// 触发 connection 事件 
eventEmitter.emit('connection');

console.log("程序执行完毕。");
接下来让我们执行以上代码：
$ node main.js
连接成功。
数据接收成功。
程序执行完毕。
Node 应用程序是如何工作的？
在 Node 应用程序中，执行异步操作的函数将回调函数作为最后一个参数， 回调函数接收错误对象作为第一个参数。
接下来让我们来重新看下前面的实例，创建一个 input.txt ,文件内容如下：
菜鸟教程官网地址：www.runoob.com
创建 main.js 文件，代码如下：
var fs = require("fs");

fs.readFile('input.txt', function (err, data) {
   if (err){
      console.log(err.stack);
      return;
   }
   console.log(data.toString());
});
console.log("程序执行完毕");
以上程序中 fs.readFile() 是异步函数用于读取文件。 如果在读取文件过程中发生错误，错误 err 对象就会输出错误信息。
如果没发生错误，readFile 跳过 err 对象的输出，文件内容就通过回调函数输出。
执行以上代码，执行结果如下：
程序执行完毕
菜鸟教程官网地址：www.runoob.com
接下来我们删除 input.txt 文件，执行结果如下所示：
程序执行完毕
Error: ENOENT, open 'input.txt'
因为文件 input.txt 不存在，所以输出了错误信息。
-------------------------------------
node.js EventEmitter Buffer Stream(类似于java IO)， node 模块系统 都是很枯燥的东西 
我们直接看node.js 函数  
这个简单
Node.js 函数
在JavaScript中，一个函数可以作为另一个函数的参数。我们可以先定义一个函数，然后传递，也可以在传递参数的地方直接定义函数。
Node.js中函数的使用与Javascript类似，举例来说，你可以这样做：
function say(word) {
  console.log(word);
}

function execute(someFunction, value) {
  someFunction(value);
}

execute(say, "Hello");
以上代码中，我们把 say 函数作为execute函数的第一个变量进行了传递。这里返回的不是 say 的返回值，而是 say 本身！
这样一来， say 就变成了execute 中的本地变量 someFunction ，execute可以通过调用 someFunction() （带括号的形式）来使用 say 函数。
当然，因为 say 有一个变量， execute 在调用 someFunction 时可以传递这样一个变量。
匿名函数
我们可以把一个函数作为变量传递。但是我们不一定要绕这个"先定义，再传递"的圈子，我们可以直接在另一个函数的括号中定义和传递这个函数：
function execute(someFunction, value) {
  someFunction(value);
}

execute(function(word){ console.log(word) }, "Hello");
我们在 execute 接受第一个参数的地方直接定义了我们准备传递给 execute 的函数。
用这种方式，我们甚至不用给这个函数起名字，这也是为什么它被叫做匿名函数 。
函数传递是如何让HTTP服务器工作的
带着这些知识，我们再来看看我们简约而不简单的HTTP服务器：
var http = require("http");

http.createServer(function(request, response) {
  response.writeHead(200, {"Content-Type": "text/plain"});
  response.write("Hello World");
  response.end();
}).listen(8888);
现在它看上去应该清晰了很多：我们向 createServer 函数传递了一个匿名函数。
用这样的代码也可以达到同样的目的：
var http = require("http");

function onRequest(request, response) {
  response.writeHead(200, {"Content-Type": "text/plain"});
  response.write("Hello World");
  response.end();
}

http.createServer(onRequest).listen(8888);

--------------------
node.js 可以做网关模块  

------上面的原理课程，底层太乏味了 ，我们直接上干货 
 黄勇 微服务架构中的node.js 源代码---------
第一个案例 app.js 
http://123.207.56.239:1234/
> 
var http = require('http');

var PORT = 1234;

var app = http.createServer(function (req, res) {
  res.writeHead(200, {'Content-Type': 'text/html'});
  console.log('server is running at %d', PORT);
  res.write('<h1>Hello111</h1>');
  res.end();
});
app.listen(PORT, function () {
  console.log('server is running at %d', PORT);
});































