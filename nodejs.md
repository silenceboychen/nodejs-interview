## nodejs面试题整理


* [什么是错误优先的回调函数](#什么是错误优先的回调函数)
* [如何避免回调地狱?](#如何避免回调地狱)
* [什么是Promise?](#什么是Promise)
* [用什么工具保证一致的代码风格？为什么要这样？](#用什么工具保证一致的代码风格为什么要这样)
* [什么是Stub?举例说明](#什么是Stub举例说明)
* [什么是测试金字塔？举例说明](#什么是测试金字塔)
* [最喜欢哪个HTTP框架？为什么？](#最喜欢哪个HTTP框架)
* [Cookies如何防范XSS攻击？](#Cookies如何防范XSS攻击)
* [如何保证依赖的安全性？](#如何保证依赖的安全性)
* [node有哪些全局对象?](#node有哪些全局对象)
* [process有哪些常用方法?](#process有哪些常用方法)
* [console有哪些常用方法?](#console有哪些常用方法)
* [node有哪些定时功能?](#node有哪些定时功能)
* [node中的事件循环是什么样子的?](#node中的事件循环是什么样子的)
* [node中的Buffer如何应用?](#node中的Buffer如何应用)
* [什么是EventEmitter?](#什么是EventEmitter)
* [如何实现一个EventEmitter?](#如何实现一个EventEmitter)
* [EventEmitter有哪些典型应用?](#EventEmitter有哪些典型应用)
* [怎么捕获EventEmitter的错误事件?](#怎么捕获EventEmitter的错误事件)
* [EventEmitter中的newListenser事件有什么用处?](#EventEmitter中的newListenser事件有什么用处)
* [什么是Stream?](#什么是Stream)
* [Stream有什么好处?](#Stream有什么好处)
* [Stream有哪些典型应用?](#Stream有哪些典型应用)
* [怎么捕获Stream的错误事件?](#怎么捕获Stream的错误事件)
* [有哪些常用Stream?分别什么时候使用?](#有哪些常用Stream分别什么时候使用)
* [实现一个Writable Stream?](#实现一个Writable-Stream)
* [内置的fs模块架构是什么样子的?](#内置的fs模块架构是什么样子的)
* [读写一个文件有多少种方法?](#读写一个文件有多少种方法)
* [fs.watch和fs.watchFile有什么区别，怎么应用?](#fswatch和fswatchFile有什么区别怎么应用)
* [怎么读取json配置文件?](#怎么读取json配置文件)
* [node的网络模块架构是什么样子的?](#node的网络模块架构是什么样子的)
* [node是怎样支持https,tls的?](#node是怎样支持https-tls的)
* [实现一个简单的http服务器?](#实现一个简单的http服务器)
* [为什么需要child-process?](#为什么需要child-process)
* [exec, execFile, spawn和fork都是做什么用的?](#exec-execFile-spawn和fork都是做什么用的)
* [实现一个简单的命令行交互程序?](#实现一个简单的命令行交互程序)
* [两个node程序之间怎样交互?](#两个node程序之间怎样交互)
* [怎样让一个js文件变得像linux命令一样可执行?](#怎样让一个js文件变得像linux命令一样可执行)
* [child-process和process的stdin, stdout, stderror是一样的吗?](child-process和process的stdin-stdout-stderror是一样的吗)
* [node中的异步和同步怎么理解?](#node中的异步和同步怎么理解)
* [有哪些方法可以进行异步流程的控制?](#有哪些方法可以进行异步流程的控制)
* [怎样绑定node程序到80端口?](#怎样绑定node程序到80端口)
* [有哪些方法可以让node程序遇到错误后自动重启?](#有哪些方法可以让node程序遇到错误后自动重启)
* [怎样充分利用多个CPU?](怎样充分利用多个CPU)
* [怎样调节node执行单元的内存大小?](#怎样调节node执行单元的内存大小)
* [程序总是崩溃，怎样找出问题在哪里?](#程序总是崩溃怎样找出问题在哪里)
* [有哪些常用方法可以防止程序崩溃?](#有哪些常用方法可以防止程序崩溃)
* [怎样调试node程序?](#怎样调试node程序)
* [如何捕获NodeJS中的错误，有几种方法?](#如何捕获NodeJS中的错误有几种方法)
* [async都有哪些常用方法，分别是怎么用?](#async都有哪些常用方法分别是怎么用)
* [express项目的目录大致是什么样子的?](#express项目的目录大致是什么样子的)
* [express常用函数](#express常用函数)
* [express中如何获取路由的参数](#express中如何获取路由的参数)
* [express的response有哪些常用方法](#express的response有哪些常用方法)
* [exports和module.exports的区别和关系?](#exports和moduleexports的区别和关系)
* [客户端禁用cookie后怎么使用session?](#客户端禁用cookie后怎么使用session)
* [Libuv是什么,他跟Node是什么关系?](#Libuv是什么他跟Node是什么关系)
* [Node.js适合用于什么开发,为什么?](#Nodejs适合用于什么开发为什么)
* [Node.js优点(特性),缺点,以及如何弥补?](#Nodejs优点特性缺点以及如何弥补)
* [事件循环的机制是怎样的](#事件循环的机制是怎样的)


### 什么是错误优先的回调函数？

错误优先的回调函数(Error-First Callback)用于同时返回错误和数据。第一个参数返回错误，并且验证它是否出错；其他参数用于返回数据。
	
```
fs.readFile(filePath, function(err, data){
	if (err){
		// 处理错误
		return console.log(err);
	}
	console.log(data);
});
```

### 如何避免回调地狱?

以下方式可以避免回调地狱:
	
1. 模块化: 将回调函数转换为独立的函数
2. 使用流程控制库，例如async
3. 使用Promise
4. 使用async/await(参考[Async/Await替代Promise的6个理由](https://blog.fundebug.com/2017/04/04/nodejs-async-await/?spm=a2c4e.11153940.blogcont226235.18.4ce015625IKdc2))


### 什么是Promise?

Promise可以帮助我们更好地处理异步操作。下面的示例中，100ms后会打印result字符串。catch用于错误处理。多个Promise可以链接起来。
	
```
new Promise((resolve, reject) => {
	setTimeout(() => {
		resolve('result');
	}, 100)
}).then(console.log)
.catch(console.error);
```

### 用什么工具保证一致的代码风格？为什么要这样？

团队协作时，保证一致的代码风格是非常重要的，这样团队成员才可以更快地修改代码，而不需要每次去适应新的风格。这些工具可以帮助我们
	
1. [ESLint](https://eslint.org/)
2. [Standard](https://standardjs.com)
3. [TSLint](https://palantir.github.io/tslint/)

### 什么是Stub?举例说明

Stub用于模拟模块的行为。测试时，Stub可以为函数调用返回模拟的结果。比如说，当我们写文件时，实际上并不需要真正去写。
	
```
var fs = require('fs');
var writeFileStub = sinon.stub(fs, 'writeFile', function(path, data, cb){
	return cb(null);
});
expect(writeFileStub).to.be.called;
writeFileStub.restore();
```

### 什么是测试金字塔?

测试金字塔反映了需要写的单元测试、集成测试以及端到端测试的比例:
![测试金字塔](https://blog.fundebug.com/2017/04/10/nodejs-interview-2017/test_pyramid.png)

测试HTTP接口时应该是这样的:

1. 很多单元测试，分别测试各个模块(依赖需要stub)
2. 较少的集成测试，测试各个模块之间的交互(依赖不能stub)
3. 少量端到端测试，去调用真正地接口(依赖不能stub)

### 最喜欢哪个HTTP框架？

这个问题根据自己的喜好回答。需要描述框架的优缺点，这样可以反映开发者对框架的熟悉程度。


### Cookies如何防范XSS攻击？

XSS(Cross-Site Scripting，跨站脚本攻击)是指攻击者在返回的HTML中插入JavaScript脚本。为了减轻这些攻击，需要在HTTP头部配置set-cookie:
	
1. ``HttpOnly`` - 这个属性可以防止``cross-site scripting``，因为它会禁止Javascript脚本访问``cookie``。
2. ``secure`` - 这个属性告诉浏览器仅在请求为HTTPS时发送``cookie``。


### 如何保证依赖的安全性？

编写Node.js应用时，很可能依赖成百上千的模块。例如，使用了Express的话，会直接依赖27个模块。因此，手动检查所有依赖是不现实的。唯一的办法是对依赖进行自动化的安全检查，有这些工具可供选择:
	
1. npm outdated
2. [Trace by RisingStack](https://trace.risingstack.com)
3. NSP
4. [GreenKeeper](https://greenkeeper.io)
5. [Snyk](https://snyk.io)

### node有哪些全局对象?

``process``, ``console``, ``Buffer``

### process有哪些常用方法?

``process.stdin``, ``process.stdout``, ``process.stderr``, 	``process.on``, ``process.env``, ``process.argv``, 	``process.arch``, ``process.platform``, ``process.exit``

### console有哪些常用方法?

``console.log/console.info``, ``console.error/console.warning``, 	``console.time/console.timeEnd``, ``console.trace``, 	``console.table``

### node有哪些定时功能?

``setTimeout/clearTimeout``, ``setInterval/clearInterval``, 	``setImmediate/clearImmediate``, ``process.nextTick``

### node中的事件循环是什么样子的?

总体上执行顺序是：``process.nextTick`` >> ``setImmidate`` >> 	``setTimeout/SetInterval``  
[参考文档](https://nodejs.org/en/docs/guides/event-loop-timers-and-nexttick/)

### node中的Buffer如何应用?

Buffer是用来处理二进制数据的，比如图片，mp3,数据库文件等.Buffer支持各种编码解码，二进制字符串互转．


### 什么是EventEmitter?

``EventEmitter``是node中一个实现观察者模式的类，主要功能是监听和发射消息，用于处理多模块交互问题.

### 如何实现一个EventEmitter?

主要分三步：定义一个子类，调用构造函数，继承``EventEmitter``
	
```
var util = require('util');
var EventEmitter = require('events').EventEmitter;

function MyEmitter() {
	EventEmitter.call(this);
} // 构造函数

util.inherits(MyEmitter, EventEmitter); // 继承

var em = new MyEmitter();
em.on('hello', function(data) {
	console.log('收到事件hello的数据:', data);
}); // 接收事件，并打印到控制台
em.emit('hello', 'EventEmitter传递消息真方便!');
```

使用ES6语法：

```
const EventEmitter = require('events');

class MyStream extends EventEmitter {
  write(data) {
    this.emit('data', data);
  }
}

const stream = new MyStream();

stream.on('data', (data) => {
  console.log(`接收的数据："${data}"`);
});
stream.write('使用 ES6');
```

### EventEmitter有哪些典型应用?

1. 模块间传递消息 
2. 回调函数内外传递消息 
3. 处理流数据，因为流是在``EventEmitter``基础上实现的. 
4. 观察者模式发射触发机制相关应用

### 怎么捕获EventEmitter的错误事件?

监听error事件即可．如果有多个EventEmitter,也可以用domain来统一处理错误事件.
	
```
var domain = require('domain');
var myDomain = domain.create();
myDomain.on('error', function(err){
	console.log('domain接收到的错误事件:', err);
}); // 接收事件并打印
myDomain.run(function(){
	var emitter1 = new MyEmitter();
	emitter1.emit('error', '错误事件来自emitter1');
	emitter2 = new MyEmitter();
	emitter2.emit('error', '错误事件来自emitter2');
});
```

### EventEmitter中的newListenser事件有什么用处?


``newListener``可以用来做事件机制的反射，特殊应用，事件管理等．当任何on事件添加到``EventEmitter``时，就会触发``newListener``事件，基于这种模式，我们可以做很多自定义处理.
	
```
var emitter3 = new MyEmitter();
emitter3.on('newListener', function(name, listener) {
	console.log("新事件的名字:", name);
	console.log("新事件的代码:", listener);
	setTimeout(function(){ 
		console.log("我是自定义延时处理机制"); 
	}, 1000);
});
emitter3.on('hello', function(){
	console.log('hello　node');
});
```

### 什么是Stream?

``stream``是基于事件``EventEmitter``的数据管理模式．由各种不同的抽象接口组成，主要包括可写，可读，可读写，可转换等几种类型．

### Stream有什么好处?

非阻塞式数据处理提升效率，片断处理节省内存，管道处理方便可扩展等.

### Stream有哪些典型应用?

文件，网络，数据转换，音频视频等.

### 怎么捕获Stream的错误事件?

监听``error``事件，方法同``EventEmitter``.

### 有哪些常用Stream?分别什么时候使用?

1. ``Readable``为可被读流，在作为输入数据源时使用；
2. ``Writable``为可被写流,在作为输出源时使用；
3. ``Duplex``为可读写流,它作为输出源接受被写入，同时又作为输入源被后面的流读出．
4. ``Transform``机制和``Duplex``一样，都是双向流，区别是``Transfrom``只需要实现一个函数``_transfrom(chunk, encoding, callback)``;而``Duplex``需要分别实现``_read(size)``函数和``_write(chunk, encoding, callback)``函数.


### 实现一个Writable Stream?


三步走:
1. 构造函数``call Writable`` 
2. 继承``Writable`` 
3. 实现``_write(chunk, encoding, callback)``函数

```
var Writable = require('stream').Writable;
var util = require('util');

function MyWritable(options) {
	Writable.call(this, options);
} // 构造函数
util.inherits(MyWritable, Writable); // 继承自Writable
MyWritable.prototype._write = function(chunk, encoding, callback) {
	console.log("被写入的数据是:", chunk.toString()); // 此处可对写入的数据进行处理
	callback();
};

process.stdin.pipe(new MyWritable()); // stdin作为输入源，MyWritable作为输出源   
```


### 内置的fs模块架构是什么样子的?

fs模块主要由下面几部分组成: 

1. ``POSIX``文件``Wrapper``,对应于操作系统的原生文件操作 
2. 文件流 ``fs.createReadStream``和``fs.createWriteStream`` 
3. 同步文件读写,``fs.readFileSync``和``fs.writeFileSync`` 
4. 异步文件读写, ``fs.readFile``和``fs.writeFile``


### 读写一个文件有多少种方法?

总体来说有四种: 

1. ``POSIX``式低层读写 
2. 流式读写 
3. 同步文件读写 
4. 异步文件读写

### fs.watch和fs.watchFile有什么区别，怎么应用?

二者主要用来监听文件变动．``fs.watch``利用操作系统原生机制来监听，可能不适用网络文件系统; ``fs.watchFile``则是定期检查文件状态变更，适用于网络文件系统，但是相比``fs.watch``有些慢，因为不是实时机制．

### 怎么读取json配置文件?

主要有两种方式，第一种是利用``node``内置的``require('data.json')``机制，直接得到js对象; 第二种是读入文件入内容，然后用``JSON.parse(content)``转换成js对象．二者的区别是``require``机制情况下，如果多个模块都加载了同一个json文件，那么其中一个改变了js对象，其它跟着改变，这是由``node``模块的缓存机制造成的，只有一个js模块对象; 第二种方式则可以随意改变加载后的js变量，而且各模块互不影响，因为他们都是独立的，是多个js对象.


### node的网络模块架构是什么样子的?


``node``全面支持各种网络服务器和客户端，包括``tcp``, ``http/https``, ``tcp``, ``udp``, ``dns``, ``tls/ssl``等.

### node是怎样支持https, tls的?

主要实现以下几个步骤即可: 

1. openssl生成公钥私钥 
2. 服务器或客户端使用https替代http 
3. 服务器或客户端加载公钥私钥证书


### 实现一个简单的http服务器?


```
var http = require('http'); // 加载http模块

http.createServer(function(req, res) {
	res.writeHead(200, {'Content-Type': 'text/html'}); // 200代表状态成功, 文档类型是给浏览器识别用的
	res.write('<meta charset="UTF-8"> <h1>我是标题啊！</h1> <font color="red">hello world</font>'); // 返回给客户端的html数据
	res.end(); // 结束输出流
}).listen(3000); // 绑定3000, 查看效果请访问 http://localhost:3000
```

### 为什么需要child-process?

``node``是异步非阻塞的，这对高并发非常有效．可是我们还有其它一些常用需求，比如和操作系统``shell``命令交互，调用可执行文件，创建子进程进行阻塞式访问或高CPU计算等，``child-process``就是为满足这些需求而生的．``child-process``顾名思义，就是把``node``阻塞的工作交给子进程去做．

### exec, execFile, spawn和fork都是做什么用的?

1. ``exec``可以用操作系统原生的方式执行各种命令，如管道 ``cat ab.txt | grep hello``; 
2. ``execFile``是执行一个文件; 
3. ``spawn``是流式和操作系统进行交互; 
4. ``fork``是两个``node``程序(``javascript``)之间时行交互.

### 实现一个简单的命令行交互程序?


```
var cp = require('child_process');

var child = cp.spawn('echo', ['你好', "钩子"]); // 执行命令
child.stdout.pipe(process.stdout); // child.stdout是输入流，process.stdout是输出流
// 这句的意思是将子进程的输出作为当前程序的输入流，然后重定向到当前程序的标准输出，即控制台
```


### 两个node程序之间怎样交互?

使用``child_process``的``fork``．原理是子程序用``process.on``, ``process.send``，父程序里用``child.on``,``child.send``进行交互.
	
```
1) fork-parent.js
var cp = require('child_process');
var child = cp.fork('./fork-child.js');
child.on('message', function(msg){
	console.log('老爸从儿子接受到数据:', msg);
});
child.send('我是你爸爸，送关怀来了!');

2) fork-child.js
process.on('message', function(msg){
	console.log("儿子从老爸接收到的数据:", msg);
	process.send("我不要关怀，我要银民币！");
});
```

### 怎样让一个js文件变得像linux命令一样可执行?

1. 在``myCommand.js``文件头部加入 ``#!/usr/bin/env node`` 
2. ``chmod``命令把js文件改为可执行即可 
3. 进入文件目录，命令行输入``myComand``就是相当于``node myComand.js``了


### child-process和process的stdin, stdout, stderror是一样的吗?

概念都是一样的，输入，输出，错误，都是流．区别是在父程序眼里，子程序的``stdout``是输入流，``stdin``是输出流．


### node中的异步和同步怎么理解?

node是单线程的，异步是通过一次次的循环事件队列来实现的．同步则是说阻塞式的IO,这在高并发环境会是一个很大的性能问题，所以同步一般只在基础框架的启动时使用，用来加载配置文件，初始化程序什么的．


### 有哪些方法可以进行异步流程的控制?

1. 多层嵌套回调 
2. 为每一个回调写单独的函数，函数里边再回调 
3. 用第三方框架比方``async, q, promise``等


### 怎样绑定node程序到80端口?

1. ``sudo`` 
2. ``apache/nginx``代理 
3. 用操作系统的``firewall iptables``进行端口重定向

### 有哪些方法可以让node程序遇到错误后自动重启?

1. ``runit`` 
2. ``forever`` 
3. ``nohup npm start &``
4. ``docker`

### 怎样充分利用多个CPU?

一个CPU运行一个node实例

### 怎样调节node执行单元的内存大小?

用``--max-old-space-size`` 和 ``--max-new-space-size`` 来设置 v8 使用内存的上限

### 程序总是崩溃，怎样找出问题在哪里?

1. ``node --prof`` 查看哪些函数调用次数多 
2. ``memwatch``和``heapdump``获得内存快照进行对比，查找内存溢出

### 有哪些常用方法可以防止程序崩溃?

1. ``try-catch-finally`` 
2. ``EventEmitter/Stream error``事件处理 
3. ``domain``统一控制 
4. ``jshint``静态检查 
5. ``jasmine/mocha``进行单元测试

### 怎样调试node程序?

``node --debug app.js`` 和``node-inspector``


### 如何捕获NodeJS中的错误，有几种方法?

1、 try catch 方式
	
```
try {
    syncError()
} catch (e) {
    /*处理异常*/
    console.log(e.message)
}
```
2、 callback方式

```
fs.mkdir('/dir', function (e) {
    if (e) {
        /*处理异常*/
        console.log(e.message)
    } else {
        console.log('创建目录成功')
    }
})
```
3、 event方式

```
let events = require("events");
//创建一个事件监听对象
let emitter = new events.EventEmitter();
//监听error事件
emitter.addListener("error", function (e) {
    /*处理异常*/
    console.log(e.message)
});
//触发error事件
emitter.emit("error", new Error('出错啦'));
```

4、 Promise方式

```
new Promise((resolve, reject) => {
    syncError()
}).then(() => {
    //...
}).catch((e) => {
    /*处理异常*/
    console.log(e.message)
})
```

5、 Async/awiat方式

Async/Await是基于Promise的，所以Promise无法捕获的异常，Async/Await同样无法捕获

6、 process方式

process方式可以捕获任何异常(不管是同步代码块中的异常还是异步代码块中的异常)

```
process.on('uncaughtException', function (e) {
    /*处理异常*/
    console.log(e.message)
});
asyncError()
syncError()
```

7、domain方式

process方式虽然可以捕获任何类型的异常，但是process太过笨重，除了记录下错误信息，其他地方不适合使用，domain这个也可以处理任何类型异常的模块，显然是一个不错的选择。

```
let domain = require('domain')
let d = domain.create()
d.on('error', function (e) {
    /*处理异常*/
    console.log(e.message)
})
d.run(asyncError)
d.run(syncError)
```


### async都有哪些常用方法，分别是怎么用?


``async``是一个js类库，它的目的是解决js中异常流程难以控制的问题．``async``不仅适用在node.js里，浏览器中也可以使用．
	
1、``async.parallel``并行执行完多个函数后，调用结束函数

```
async.parallel([
    function(){ ... },
    function(){ ... }
], callback);
```

async.series串行执行完多个函数后，调用结束函数

```
async.series([
    function(){ ... },
    function(){ ... }
]);
```

2、``async.waterfall``依次执行多个函数，后一个函数以前面函数的结果作为输入参数

```
async.waterfall([
    function(callback) {
        callback(null, 'one', 'two');
    },
    function(arg1, arg2, callback) {
      // arg1 now equals 'one' and arg2 now equals 'two'
        callback(null, 'three');
    },
    function(arg1, callback) {
        // arg1 now equals 'three'
        callback(null, 'done');
    }
], function (err, result) {
    // result now equals 'done'
});
```

3、``async.map``异步执行多个数组，返回结果数组

```
async.map(['file1','file2','file3'], fs.stat, function(err, results){
    // results is now an array of stats for each file
});
```

4、``async.filter``异步过滤多个数组，返回结果数组

```
async.filter(['file1','file2','file3'], fs.exists, function(results){
    // results now equals an array of the existing files
});
```

### express项目的目录大致是什么样子的?

``app.js, package.json, bin/www, public, routes, views``

### express常用函数

1. ``express.Router``路由组件
2. ``app.get``路由定向
3. ``app.configure``配置
4. ``app.set``设定参数
5. ``app.use``使用中间件


### express中如何获取路由的参数

1. ``req.params``
2. ``req.body``
3. ``req.query``


### express的response有哪些常用方法

1. res.download()	弹出文件下载
2. res.end()	结束response
3. res.json()	返回json
4. res.jsonp()	返回jsonp
5. res.redirect()	重定向请求
6. res.render()	渲染模板
7. res.send()	返回多种形式数据
8. res.sendFile	返回文件
9. res.sendStatus()	返回状态


### exports和module.exports的区别和关系?

[详解](https://www.silenceboy.com/2019/03/26/%E8%BD%AC-Node-js%E7%9A%84%E6%A8%A1%E5%9D%97-exports%E5%92%8Cmodule-exports/)

用一句话来说明就是，require方能看到的只有``module.exports``这个对象，它是看不到``exports``对象的，而我们在编写模块时用到的``exports``对象实际上只是对``module.exports``的引用。
	
1. module.exports 初始值为一个空对象 {}
2. exports 是指向的 module.exports 的引用
3. require() 返回的是 module.exports 而不是 exports

所有的 ``exports`` 做的工作实际上是收集属性，如果 ``module.exports`` 当前没有任何属性，``exports``便将收集到的属性添加到 ``module.exports`` 上。如果 ``module.exports``已经存在若干属性，``exports`` 上的属性都会被忽略。


### 客户端禁用cookie后怎么使用session?

1. URL重写，就是把``session id``直接附加在URL路径的后面
2. 表单隐藏字段。就是服务器会自动修改表单，添加一个隐藏字段，以便在表单提交时能够把``session id``传递回服务器。


### Libuv是什么,他跟Node是什么关系?

libuv是一个高性能的，事件驱动的I/O库，并且提供了跨平台（如windows, linux）的API。libuv强制使用异步的，事件驱动的编程风格。它的核心工作是提供一个event-loop，还有基于I/O和其它事件通知的回调函数。

nodejs使用libuv监听各种异步事件

### Node.js适合用于什么开发,为什么?

Node.js适合做一些高并发的，I/O密集型的应用。

当应用程序需要处理大量并发的I/O，而在向客户端发出响应之前，应用程序内部并不需要进行非常复杂的处理的时候，Node.js非常适合。Node.js也非常适合与web socket配合，开发长连接的实时交互应用程序。

### Node.js优点(特性),缺点,以及如何弥补?

**Node.js优点：**
	
1、采用事件驱动、异步编程，为网络服务而设计。其实Javascript的匿名函数和闭包特性非常适合事件驱动、异步编程。而且JavaScript也简单易学，很多前端设计人员可以很快上手做后端设计。

2、Node.js非阻塞模式的IO处理给Node.js带来在相对低系统资源耗用下的高性能与出众的负载能力，非常适合用作依赖其它IO资源的中间层服务。

3、Node.js轻量高效，可以认为是数据密集型分布式部署环境下的实时应用系统的完美解决方案。Node非常适合如下情况：在响应客户端之前，您预计可能有很高的流量，但所需的服务器端逻辑和处理不一定很多。

4、单进程，节约资源

5、异步I/O，提升并发量

**Node.js缺点：**

1、可靠性低, 一旦出现未捕获的异常将直接导致服务不可用.

2、单进程，单线程，只支持单核CPU，不能充分的利用多核CPU服务器。一旦这个进程崩掉，那么整个web服务就崩掉了。

不过以上缺点可以可以通过代码的健壮性来弥补。使用cluster模式开启多个进程。
设置全局异常捕获器。

**事件循环的机制是怎样的**

1. timers: 这个阶段执行定时器队列中的回调如 setTimeout() 和 setInterval()。
2. I/O callbacks: 这个阶段执行几乎所有的回调。但是不包括close事件，定时器和setImmediate()的回调。
3. idle, prepare: 这个阶段仅在内部使用，可以不必理会。
4. poll: 等待新的I/O事件，node在一些特殊情况下会阻塞在这里。
5. check: setImmediate()的回调会在这个阶段执行。
6. close callbacks: 例如socket.on('close', ...)这种close事件的回调。