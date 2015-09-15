# 14、2015-08-18
* mongoose update数据时，如果有数据为undified，则不能更新成功
  * 会报一下错误

* 用数组队列的方法，实现请求网络api获取数据，然后更新
  * 为了控制内存，除了要控制数组的长度，还要控制处于请求中的对象数量

* 继承的使用方法
    Util.inherits(Connection, EventEmitter);
    function Connection(options) {
      EventEmitter.call(this);

      this.config = options.config;

      this._socket        = options.socket;
      this._protocol      = new Protocol({config: this.config, connection: this});
      this._connectCalled = false;
      this.state          = "disconnected";
      this.threadId       = null;
    }

# 13、2015-08-16
* 创建了一个ExpiredReward函数，可以用new的方式创建这个函数的对象，准备将这个函数的prototype上的函数做为async的参数
  结果发现，async调用这个函数时，this的指向就变了，应该是指向了async函数，为了解决这个问题，我把这个函数包在了，定义ExpiredReward
  对象函数的一个子函数中，这样讲包装函数传给async，这样就不会有问题了，示例代码如下：
  function c() {
    var expiredreward = new ExpiredReward(topic);
    function a(callback) {
        expiredreward.getReplies(callback);
    }

    async.waterfall([a], function(err) {
        debug(start, sessionid, 'err:', err);
        debug(start, sessionid, 'data:', JSON.stringify(expiredreward.data));
    });
  }

# 12、2015-08-14
* mysql的连接池generic-pool的使用记录
  * 程序启动时不建立和mysql的连接，当有请求时，会建立
  * idleTimeoutMillis，表示的是轮询是否有连接的时间间隔，当mysql重启后，连接中断，在这段时间内会重新建立连接

# 11、2015-08-13
* express的路由use的链式调用，当最后一个执行完后，没有函数了，但是执行了next，会报如下错误：
  Thu Aug 13 2015 17:15:39 GMT+0800 (CST) uncaughtException Error: Can't set headers after they are sent. Error: Can't set headers after they are sent.

  > express

# 10、2015-08-05
* 如果要做轮询用户，然后做一些处理，而且要限定速度
  * 这种情况可以使用produce/consume模型来做，produce查询用户，然后将用户放到数组队列中，consume来一个一个消费队列中的数据

# 9、2015-06-26
* node-inspector使用
  * installation
    npm install -g node-inspector

    //发现在ubuntu 14.04上安装不成功，报如下错误：
    sh: 1: node-gyp: Permission denied

    在centos 6.4上可以，我将centos上安装的复制到ubuntu上，可以使用，需要执行：
    cd /root/workspace/bin/node/lib/node_modules
    tar -zxvf /root/workspace/tools/node-inspector.tar.gz
    cd /usr/bin
    ln -s /root/workspace/bin/node/lib/node_modules/node-inspector/bin/inspector.js node-inspector
    ln -s /root/workspace/bin/node/lib/node_modules/node-inspector/bin/node-debug.js node-debug

  * how to use
    * node-inspector --web-port=3000
    * node debug app.js

# 8、2015-06-25
* 使用request调用环信接口，需要使用raw格式的数据

  var options = {
      url: hotauto.easemob.url + 'users',
      headers: {
          "Authorization": "Bearer " + token
      },
      body: JSON.stringify({"username": usernameMd5, "password": passwordMd5})
  };

  主要是body部分，不使用form，而用body，内容转换一下

# 7、2015-06-16
* __dirname
      {String}
      The name of the directory that the currently executing script resides in.

      Example: running node example.js from /Users/mjr

      console.log(__dirname);
      // /Users/mjr
  __dirname isn't actually a global but rather local to each module.

* 如果不在程序所在目录启动的话，修改给路径前加上__dirname

      cd /root
      /root/bin/node/bin/node /root/monitor/app.js dongfong

  以静态文件为例：
      app.use(express.static(__dirname + '/public'));
      app.use(express.static(__dirname + '/bower_components'));

# 6、2015-05-21
[Growing Up](https://medium.com/node-js-javascript/growing-up-27d6cc8b7c53)

  io.js needs a foundation.

  keywords: nodejs iojs

* [Node.js与io.js那些事儿](http://www.infoq.com/cn/articles/node-js-and-io-js)

  去年12月，多位重量级Node.js开发者不满Joyent对Node.js的管理，自立门户创建了io.js。io.js的发展速度非常快，先是于2015年1月份发布了1.0版本，并且很快就达到了2.0版本，社区非常活跃。而最近io.js社区又宣布，这两个项目将合并到Node基金会下，并暂时由“Node.js和io.js核心技术团队联合监督”运营。本文将聊一聊Node.js项目的一些历史情况，与io.js项目之间的恩怨纠葛，他们将来的发展去向。希望能从历史的层面去了解这个开源项目在运营模式上是如何演变和发展的。

  > keywords: node.js io.js

* [朴灵：打破限制，从前端到全栈（图灵访谈）](http://www.ituring.com.cn/article/197773)

  当时我们团队的Leader不会去限制工程师做什么事情，而是说这件事是你在负责，所以你就要从前做到后。

  > keywords: nodejs javascript

# 5、2015-05-19
* [客户端session vs 服务端session](http://snoopyxdy.blog.163.com/blog/static/60117440201451095858254/)

  session想来大家都不会陌生，session主要的作用就是用来记录用户状态，表明用户身份的，session的存储方式也有多样，最为传统的就是服务端保存session的内容，客户端浏览器cookie保存sessionid，服务端通过客户端每次http请求带上的cookie中的sessionid去找到对应此用户的session内容。当然我之前也发过一篇文章讲到过通过etag来做为sessionid，识别用户身份。相关文章链接：http://cnodejs.org/topic/5212d82d0a746c580b43d948

  > keywords: session nodejs python flask base64 cookie

# 4、2015-05-10
## 4.2、[从零开始nodejs系列文章](http://blog.fens.me/series-nodejs/)
    从零开始nodejs系列文章，将介绍如何利Javascript做为服务端脚本，通过Nodejs框架web开发。Nodejs框架是基于V8的引擎，是目前速度最快的Javascript引擎。chrome浏览器就基于V8，同时打开20-30个网页都很流畅。Nodejs标准的web开发框架Express，可以帮助我们迅速建立web站点，比起PHP的开发效率更高，而且学习曲线更低。非常适合小型网站，个性化网站，我们自己的Geek网站！！

## 4.1、使用events实现发布/订阅
    var events = require("events");
    var emitter = new events.EventEmitter();

    // 发布
    emitter.emit('connection', "socket", socket);

    // 订阅
    emitter.on("connection", function (key, value) {
        console.log(queryChannel, "client connect the server.", value);
        socket = value;
    });

# 3、2015-04-30
* [WordPress 4.3 will be rewritten in Node.js](http://webuilddesign.com/wordpress-4-3-will-be-rewritten-in-node-js/?utm_source=ourjs.com)
* [WordPress 4.3核心功能将放弃PHP并使用Node.JS重写](http://ourjs.com/detail/5540d751329934463f000005)

# 2、2015-04-29
* [Node.js Module – exports vs module.exports](http://www.hacksparrow.com/node-js-exports-vs-module-exports.html)
* []()

# 1、2015-04-28
* [NodeJS异常处理uncaughtException篇](http://code.oneapm.com/nodejs/2015/04/08/nodejs-uncaughtexception/)
* [Error Handling in Node.js](https://www.joyent.com/developers/node/design/errors)
* [翻译 - NodeJS错误处理最佳实践](http://code.oneapm.com/nodejs/2015/04/13/nodejs-errorhandling/)
* [Node.js](https://nodejs.org/)
* [Node.js异步编程](http://airjd.com/view/i8ygju2z000b6kl#1)
