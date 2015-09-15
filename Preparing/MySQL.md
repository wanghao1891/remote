# 4、2015-08-24
* mysql存储过程
  DELIMITER //
  DROP PROCEDURE IF EXISTS get_artcoin_info;
  CREATE PROCEDURE get_artcoin_info()
  BEGIN
      -- Do SQL Operation
      select id, userid, money, name, resourceid, consume_artcoin_id, type, consumeStatus, artCoinStatus from product_order where userid in ('544623751fe3118138e14b13', '5439459d8a53120d43356c50') order by id;
      select * from money where userid in ('544623751fe3118138e14b13', '5439459d8a53120d43356c50') order by id;
  END
  //
  DELIMITER ;

  * 注意
    结尾的 DELIMITER 和 ; 之间，必须要有空格

* mysql变量的使用
  set @_artcoinid=1914;
  insert into product_order(userid, type, money, orderid, consumeStatus, resourceid, consume_artcoin_id, name) value('544623751fe3118138e14b13', 5, 2000, '2015080114340000000000123', 1, '55da8ce6e89c0d6a00847a2a', @_artcoinid, '在线视频');
  update product_order set artCoinStatus=1 where id=@_artcoinid;
  update money set total_money=total_money-2000 where userid='544623751fe3118138e14b13';

# 3、2015-08-04
* 当记录不存在是插入，存在时更新
  replace into money(userid, total_money) values('5513c0d4596f70cf5dc6bd23', 200);

# 2、2015-08-01
* 查看注释信息
  show full columns from product_order;

# 1、2015-07-24
* Increment value in mysql update query

  update money set total=total-5 where userid='543779d40d819a5f6abeda12';

* [MYSQL的事务处理功能](http://www.cnblogs.com/qiantuwuliang/archive/2010/11/02/1867291.html)

  事务处理在各种管理系统中都有着广泛的应用，比如人员管理系统，很多同步数据库操作大都需要用到事务处理。比如说，在人员管理系统中，你删除一个人员，你即需要删除人员的基本资料，也要删除和该人员相关的信息，如信箱，文章等等，这样，这些数据库操作语句就构成一个事务！

  > transaction

* [关于MySQL事务的操作示例以及注意事项](http://database.51cto.com/art/201107/277732.htm)

  事务的特征：

  * Atomicity(原子性)
  * Consistency(稳定性,一致性)
  * Isolation(隔离性)
  * Durability(可靠性)
  * 注意：事务只针对对数据数据产生影响的语句有效。

  > transaction
