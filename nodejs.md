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







