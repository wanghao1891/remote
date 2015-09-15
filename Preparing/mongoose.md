# 6、2015-07-24
* 如果想给mongoose返回的对象中增加新的属性，需要先调用toObject()函数
  * 个人觉得是把该对象中的mongoose封装的内容去掉了。
  * 原有的对象可以修改

# 5、2015-06-05
* 如果要在查询返回的数据对象中插入新的属性，需要先toObject()

      var _user = user.toObject();
      _user.issignin = true;

# 4、2015-05-28
* 当连接配置为replication时，对secondary的访问是负载均衡的，配置如下：

      mongoose.connect("mongodb://192.168.56.2:20000/meishubaoline,mongodb://192.168.56.2:22000/meishubaoline,mongodb://192.168.56.2:26000/meishubaoline", {replset: { rs_name: 'rs0' }}, function (err) {
              if (err) {
      			console.log(err );
                  logger.error('connect to mongodb replicaset error: ', err.message);
                  process.exit(1);
              } else {
      			console.log("connect");
                  logger.info('connect to mongodb replicaset success');
              }
      });

# 3、2015-05-12
* [关于NodeJs为什么要用mongoose操作mongodb](https://cnodejs.org/topic/50c145ed637ffa4155c7eaee)
* [Mongoose学习参考文档——基础篇](https://cnodejs.org/topic/504b4924e2b84515770103dd?utm_source=ourjs.com)

# 2、2015-04-11
* [Schemas](http://mongoosejs.com/docs/guide.html)

# 1、2015-04-08
* [mongoose](http://mongoosejs.com/)
* [Mongoose学习参考文档](https://cnodejs.org/topic/504b4924e2b84515770103dd)
* [Getting Started](http://mongoosejs.com/docs/index.html)
