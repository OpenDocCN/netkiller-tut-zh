# 部分 II. Database Modeling Design

## RDBMS / ORDBMS / OODBMS / HDMS 数据库设计

下面数据库设计实例中，大部分使用 MySQL,PostgreSQL 为例,少部分以 Oracle 为例。

## 第 4 章 关系型数据库设计

### *MySQL 数据库设计案例*

## 数据字典

我不建议使用传统的《数据字典》，我的做法是 E-R 图加数据库注释

注释伴随表，视图，触发器，过程等等，便于维护

## 用户帐号表

用户帐号或通行证系统设计，下面以我的数库为例讲解。

我一般使用两个表 passport，profile 完成网站会员系统。

首先说说 passport 表，你也要以使用 user 或 member 等等命名，这个表设计尽可能地简单，不要使用过多字段。仅保存登录所必须用到的字段，如 user,password,nickname,email... 登录帐号和密码做复合索引。

然后是 profile 表，这个表与 passport 是 1:1 关系，保存用户详细信息

这样设计可以保证海量用户登录时的速度。

```

+----------+
| user     |
|----------|
|id        | <---+
|user      |     |
|passwd    |     |
|nickname  |     |
|status    |     |
+----------+     |
                1:1
+----------+     |
| profile  |     |
|----------|     |
|user_id   | o---+
|name      |
|sex       |
|address   |
|telphone  |
|status    |
+----------+

```

### 用户注册键盘跟踪表设计

该表的功能是，防止用户注册过程中流逝，记录已经填写的数据。

```

CREATE TABLE `signup_keyloggers` (
	`id` INT(10) UNSIGNED NOT NULL AUTO_INCREMENT COMMENT '唯一 ID',
	`cookie` VARCHAR(32) NOT NULL COMMENT 'cookie id',
	`type` ENUM('baidu','google') NOT NULL COMMENT '推广账号类型',
	`field` ENUM('Name','Mobile','Email') NOT NULL COMMENT '字段名',
	`value` VARCHAR(50) NOT NULL COMMENT '值',
	`status` ENUM('New','Sent','Ignored','Called','Processed') NOT NULL DEFAULT 'New' COMMENT '状态',
	`operator` VARCHAR(10) NOT NULL COMMENT '操作人',
	`ctime` TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
	`mtime` TIMESTAMP NOT NULL DEFAULT '0000-00-00 00:00:00' COMMENT '状态修改时间',
	PRIMARY KEY (`id`),
	UNIQUE INDEX `unique_index` (`type`, `cookie`, `field`, `value`)
)
COMMENT='用户注册键盘记录器'
COLLATE='utf8_general_ci'
ENGINE=InnoDB;

```

当用户注册成功会根据 cookie id 删除该表中的数据。

当数据被记录后，客服就可以对客户回访，并修改状态 status，忽略 Ignored，邮件发送 Sent， 电话回访 Called 等等

## 分类表设计

### 树形分类表

```

 +-----------+
 | category  |
 |-----------|
 |id         | <---+
 |title      |     |
 |description|    1:n
 |status     |     |
 |parent_id  | o---+
 +-----------+

```

```

CREATE TABLE `category` (
	`id` SMALLINT(10) UNSIGNED NOT NULL AUTO_INCREMENT,
	`name` VARCHAR(10) NOT NULL,
	`description` VARCHAR(255) NULL,
	`status` ENUM('enable','desable') NOT NULL DEFAULT 'enable',
	`parent_id` SMALLINT(10) UNSIGNED NOT NULL DEFAULT '0',
	PRIMARY KEY (`id`),
	CONSTRAINT `FK1` FOREIGN KEY (`parent_id`) REFERENCES `category` (`id`)
)
COMMENT='goods category'
ENGINE=InnoDB
ROW_FORMAT=DEFAULT

```

### 多对多分类

多对多分类,主要用于满足，一个产品/文章属于多个分类的需求。

```

      +------------+
      | category   |
      |------------|
 +--> |id          | <---+
 |    |title       |     |     +----------------------+
1:n   |description |    1:n    | categroy_has_product |
 |    |status      |     |     +----------------------+
 +--o |parent_id   |     |     | id                   |
      +------------+     +---o | category_id          |
                         +---o | product_id           |
      +------------+     |     +----------------------+
      | product    |    1:n
      +------------+     |
      |id          | <---+
      |price       |
      |quantity    |
      |...         |
      |status      |
      +------------+

```

### 快速检索子分类设计

上面我刚刚讲过怎样实现“不限子树的分类树”，我们可以实现不限层次的无线分类表。

```

 +-----------+
 | category  |
 |-----------|
 |id         | <---+
 |title      |     |
 |description|    1:n
 |status     |     |
 |parent_id  | o---+
 +-----------+

```

问题出来了，当我需要读取一个分类（任意分类）下的所有子分类，怎样实现，很多人会说用“递归”。 当然“递归”可是现实我们的需求，在几百个分类的项目中，使用递归也不是不可以的，但是当数量非常庞大时怎么办？

当然有更好的解决方案，请看下面

```

 +-----------+
 | category  |
 |-----------|
 |id         | <---+
 |title      |     |
 |description|    1:n
 |status     |     |
 |parent_id  | o---+
 |path       |
 +-----------+

```

```

+-------------------------------------------------------------------------+
| category                                                                |
+----+-----------+-----------------------+--------+-----------+-----------+
| id | name      | description           | status | parent_id | path      |
+----+-----------+-----------------------+--------+-----------+-----------+
|  1 | 中国    | 中华人民共和家                                    | Y      |      NULL | 1/        |
|  4 | 广东省 | 广东省                                                      | Y      |         1 | 1/4       |
|  5 | 深圳市 | NULL                      | Y      |         4 | 1/4/5     |
|  6 | 宝安区 | NULL                      | Y      |         5 | 1/4/5/6   |
|  7 | 龙华镇 | NULL                      | Y      |         6 | 1/4/5/6/7 |
+----+-----------+-----------------------+--------+-----------+-----------+

```

```

CREATE TABLE `category` (
	`id` INT(10) UNSIGNED NOT NULL AUTO_INCREMENT COMMENT '分类 ID',
	`name` VARCHAR(50) NOT NULL COMMENT '分类名称',
	`description` VARCHAR(200) NULL DEFAULT NULL COMMENT '分类描述',
	`status` ENUM('Y','N') NOT NULL DEFAULT 'Y' COMMENT '分类状态有继承性',
	`parent_id` INT(10) NULL DEFAULT '1' COMMENT '分类父 ID',
	`path` VARCHAR(255) NOT NULL COMMENT '分类递归路径索引',
	INDEX `PK` (`id`),
	INDEX `relation` (`id`, `parent_id`),
	INDEX `FK_category_category` (`parent_id`),
	INDEX `path` (`path`)
)
COMMENT='分类表'
ENGINE=InnoDB
ROW_FORMAT=DEFAULT
AUTO_INCREMENT=0

insert into category(`name`,`description`,`status`,`parent_id`,`path`) values('中国','中华人民共和家','Y',null,'1/')

```

```

ALTER TABLE `category`
	ADD CONSTRAINT `FK_category_category` FOREIGN KEY (`parent_id`) REFERENCES `category` (`id`)

```

抽取广东子树

```

select * from category where path like '1/4%';

```

```

mysql> select * from category where path like '1/4%';
+----+-----------+-------------+--------+-----------+-----------+
| id | name      | description | status | parent_id | path      |
+----+-----------+-------------+--------+-----------+-----------+
|  4 | 广东省 | 广东省   | Y      |         1 | 1/4       |
|  5 | 深圳市 | NULL        | Y      |         4 | 1/4/5     |
|  6 | 宝安区 | NULL        | Y      |         5 | 1/4/5/6   |
|  7 | 龙华镇 | NULL        | Y      |         6 | 1/4/5/6/7 |
+----+-----------+-------------+--------+-----------+-----------+
4 rows in set (0.00 sec)

```

### 计算节点数量

```

DROP TABLE IF EXISTS `test`;
CREATE TABLE IF NOT EXISTS `test` (
  `id` int(11) DEFAULT NULL,
  `pid` int(11) DEFAULT NULL,
  `name` char(50) DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

INSERT INTO `test` (`id`, `pid`, `name`) VALUES
	(1, 0, 'A'),
	(2, 1, 'B'),
	(3, 1, 'C'),
	(4, 0, 'D'),
	(5, 0, 'E'),
	(6, 5, 'F');

select (select t2.name from test t2 where t2.id=t1.pid) as name, count(pid) as sum from test t1 where t1.pid <> 0 group by t1.pid;

```

统计所有节点包括数量为零的

```
select t1.name, (select count(t2.name) from test t2 where t2.pid=t1.id) as sum from test t1

```

### Example

例 4.1. identity_card 身份证归属地表

```
CREATE TABLE `identity_card` (
	`id` INT(10) UNSIGNED NOT NULL AUTO_INCREMENT COMMENT '唯一主键',
	`pid` INT(10) UNSIGNED NOT NULL DEFAULT '0' COMMENT '父 ID',
	`path` VARCHAR(50) NOT NULL COMMENT '路径',
	`number` VARCHAR(18) NOT NULL COMMENT '身份证号码段',
	`zone` VARCHAR(50) NOT NULL COMMENT '行政区域',
	`status` ENUM('Y','N') NOT NULL DEFAULT 'N' COMMENT '状态',
	`modified` TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '创建与修改时间',
	PRIMARY KEY (`id`),
	INDEX `FK_identity_card_identity_card` (`pid`),
	INDEX `path` (`path`),
	INDEX `number` (`number`),
	CONSTRAINT `FK_identity_card_identity_card` FOREIGN KEY (`pid`) REFERENCES `identity_card` (`id`) ON UPDATE CASCADE ON DELETE CASCADE
)
COMMENT='identity card number'
COLLATE='utf8_general_ci'
ENGINE=InnoDB;

```

```
"id"	"pid"	"path"	"number"	"zone"	"status"	"modified"
"1012"	"1"	"1.1012"	"330000"	"浙江省"	"Y"	"2012-05-16 17:18:14"
"1041"	"1012"	"1.1012.1041"	"330300"	"温州市"	"Y"	"2012-05-16 17:44:18"
"1052"	"1041"	"1.1012.1041.1052"	"330381"	"瑞安市"	"Y"	"2012-05-16 17:44:25"
"1367"	"1"	"1.1367"	"360000"	"江西省"	"Y"	"2012-05-16 16:57:23"
"1451"	"1367"	"1.1367.1451"	"360900"	"宜春市"	"Y"	"2012-05-16 17:44:58"
"1990"	"1"	"1.1990"	"430000"	"湖南省"	"Y"	"2012-05-16 16:50:50"
"1991"	"1990"	"1.1990.1991"	"430100"	"长沙市"	"Y"	"2012-05-16 16:50:54"
"2124"	"1990"	"1.1990.2124"	"431300"	"娄底市"	"Y"	"2012-05-16 16:54:45"

```

## 文章表设计

看具体情况，拆分表，可按“日”，“月”，“年”等等

```

      +-----------+
      | category  |
      |-----------|
  +-->|id         | <---+
  |   |title      |     |
  |   |description|    1:n
  |   |status     |     |
  |   |parent_id  | o---+
  |   +-----------+
  |
 1:n
  |
  |   +-----------------+            +------------------+
  |   | article_2008_01 |            | feedback_2008_01 |
  |   |-----------------|            |------------------|
  |   |id               |<--1:n--+   |id                |
  |   |title            |        |   |title             |
  |   |content          |        |   |content           |
  |   |datetime         |        |   |datetime          |
  |   |status           |        |   |status            |
  +--o|category_id      |        +--o|news_id           |
  +--o|user_id          |        +-->|user_id           |
  |   +-----------------+        |   +------------------+
  |                              |
 1:n  +----------+     +---1:n---+
  |   | user     |     |
  |   |----------|     |
  +-->|id        | <---+
      |user      |
      |passwd    |
      |nickname  |
      |status    |
      +----------+

```

### 分区表设计

分区表可以通过表空间，等等技术实现，优点是解决了 Union 查询问题，保证了数据的一致性。

```

      +-----------+
      | category  |
      |-----------|
  +-->|id         | <---+
  |   |title      |     |
  |   |description|    1:n
  |   |status     |     |
  |   |parent_id  | o---+
  |   +-----------+
  |
 1:n
  |
  |   +-----------------+            +-----------------+
  |   | article         |            | feedback        |
  |   |-----------------|            |-----------------|
  |   |id               |<--1:n--+   |id               |
  |   |title            |        |   |title            |
  |   |content          |        |   |content          |
  |   |datetime         |        |   |datetime         |
  |   |status           |        |   |status           |
  +--o|category_id      |        +--o|news_id          |
  +--o|user_id          |        +-->|user_id          |
  |   +-----------------+        |   +-----------------+
  |   | 2007,2008,2009  |        |   | 2007,2008,2009  |
  |   +-----------------+        |   +-----------------+
  |                              |
 1:n  +----------+     +---1:n---+
  |   | user     |     |
  |   |----------|     |
  +-->|id        | <---+
      |user      |
      |passwd    |
      |nickname  |
      |status    |
      +----------+

```

### Title 性能优化

显示 title 前 20 个汉字并在后尾添加省略号。

## 评论表

```

+----------+
| user     |
|----------|
|id        | <---+
|user      |     |
|passwd    |     |
|nickname  |     |
|status    |     |
+----------+     |
                1:n
+-----------+    |      +-----------+
| feedback  |    |      | news      |
|-----------|    |      |-----------|
|id         |    |  +-->|id         |
|title      |    |  |   |title      |
|content    |    |  |   |content    |
|datetime   |    | 1:n  |datetime   |
|status     |    |  |   |status     |
|user_id    |o---+  |   |user_id    |
|news_id    |o------+   +-----------+
+-----------+

```

## 记录点击率，阅读次数，及评分表

```

+--------------+             +--------------+
| article      |             | article_rank |
|--------------|             |--------------|
|id            | <---1:1---o |article_id    |
|title         |             |click         |
|content       |             |read          |
|datetime      |             |score         |
|status        |             |...           |
|category_id   |             |...           |
|user_id       |             |...           |
+--------------+             +--------------+

```

## 产品属性表

### 简单实现

```

+------------+           +--------------------------+           +-----------------------+
| product    |           | product_attribute        |           |product_attribute_key  |
+------------+           +--------------------------+           +-----------------------+
|id          | <--1:1--o |product_id                |     +---> |id                     |
|price       |           |product_attribute_key_id  | o---+     |name                   |
|quantity    |           |product_attribute_value_id| o---+     +-----------------------+
|...         |           +--------------------------+     |     +-----------------------+
|category_id |                                           1:n    |product_attribute_value|
+------------+                                            |     +-----------------------+
                                                          +---> |id                     |
                                                                |name                   |
                                                                +-----------------------+

```

### 实现属性组管理

product attribute group

```

+------------+           +--------------------------+           +--------------------------+           +-----------------------+
| category   |           | product_attribute_group  |           | product_attribute        |           |product_attribute_key  |
+------------+           +--------------------------+           +--------------------------+           +-----------------------+
|id          |     +---> |id                        | <--1:n--o |product_attribute_group_id|     +---> |id                     |
|title       |     |     |name                      |           |product_attribute_key_id  | o---+     |name                   |
|description |    1:1    |status                    |           |product_attribute_value_id| o---+     +-----------------------+
|status      |     |     +--------------------------+           +--------------------------+     |     +-----------------------+
|parent_id   |     |                                                                            1:n    |product_attribute_value|
|default_pag | o---+                                                                             |     +-----------------------+
+------------+                                                                                   +---> |id                     |
                                                                                                       |name                   |
                                                                                                       +-----------------------+

```

### 可编辑属表

product attribute group

```

    +------------+           +------------------+           +--------------------------+           +---------------------------------+
    | category   |           | attribute_group  |           | group_has_attribute      |           |attribute_key                    |
    +------------+           +------------------+           +--------------------------+           +---------------------------------+
 +->|id          |     +-->  |id                | <--1:n--o |attribute_group_id        |     +-+-> |id                               |
 |  |title       |     |     |name              |           |attribute_key_id          | o---+ |   |name                             |
 |  |description |    1:1    |status            |           |                          |       |   |type enum('Bool','List','Input') |
 |  |status      |     |     +------------------+           +--------------------------+       |   |default array()                  |
 |  |parent_id   |     |                                                                       |   +---------------------------------+
 |  |default_pag | o---+                                                                       |
 |  +------------+                                                                             |
1:n                                                                                            |
 |  +-------------+             +--------------------------+                                   |
 |  | product     |             | product_attribute        |                                   |
 |  +-------------+             +--------------------------+                                   |
 |  |id           |         +-> |product_id                |                                   |
 |  |price        |         |   |attribute_key_id          | o---------------1:n---------------+
 |  |quantity     |         |   |attribute_value           |
 |  |...          |         |   +--------------------------+
 +-o|category_id  |         |
    |attr_group_id| <--1:n--o
    +-------------+

```

```
+--------------------------------------------------+
| product_attribute_key                            |
+--------------------------------------------------+
| 1 | color | list | red,green,blue                |
| 2 | sex   | bool | Female,Male                   |
| 3 | qty   | input| ''                            |
+--------------------------------------------------+

```

## 商品库存表

```

+------------+              +----------------+               +------------------+
| product    |              | product_store  |               | user_order       |
+------------+              +----------------+               +------------------+
|id          | <--+         |id              | <---+         |id                |
|price       |    +--1:1--o |product_id      |     |         |user_id           |
|quantity    |              |sn              |     +--1:n--o |product_store_id  |
|...         |              |status          |               |                  |
|category_id |              +----------------+               +------------------+
+------------+

```

product 是产品表总表，product_store 每个产品一条记录，同时将 sn 编号对应到物理产品，这时记录库存需要

```
select count(id) from product_store where product_id='xxxxx' and status = 'sell'

```

商品销售

```
begin;
select id from product_store where status = 'sale' and product_id='xxxxx' for update;
insert into user_order(user_id,product_store_id) values('xxxxxx','xxxxx');
update product_store set status = 'sold' where status = 'sale' and product_id='xxxxx';
commit;

```

售出的商品与用户的订单项一一对应的。

注意上面，这里使用了排它锁与事务处理，防止一个商品卖给两个人。

根据上面的思路我们可以将商品属性与 product_store 表进行一对一匹配，这样每个商品都有它自己的商品属性，甚至价格也可以移到 product_store 表中，例如不同颜色售价不同。

## 国际化语言表

```

      +-----------+             +---------------+
      | category  |    .---+    | category_lang |
      |-----------|   /    |    +---------------+
  +-->|id         | <---+  +--o |category_id    |
  |   |title      |     |       |language_id    | o---+
  |   |description|    1:n      |name           |     |      +-------------+
  |   |status     |     |       +---------------+     .      | language    |
  |   |parent_id  | o---+                              \     +-------------+
  |   +-----------+                                     >--> |id           |
 1:n                                                   /     |lang         |
  |   +------------+                                  '      |status       |
  |   | product    |                                  |      +-------------+
  |   +------------+           +--------------+       |
  |   |id          | <---+     | product_lang |       |
  |   |price       |     |     +--------------+       |
  |   |quantity    |     +---o |product_id    |       |
  |   |...         |           |language_id   | o-----+
  +-o |category_id |           |name          |
      +------------+           +--------------+

```

## Workflow

```

   +------------+     +---------------+     +-----------+
   | user       |     | role_has_user |     | role      |
   +------------+     +---------------+     +-----------+
   |id          |o-+  |id             |  +->|id         |<-+
   |node_id     |  +->|user_id        |  |  |name       |  |
   |up_id       |     |role_id        |o-+  |description|  |
   +------------+	  +---------------+     +-----------+  |
		                                                   |
   +----------------+     +------------+                   |
   | workflow       |     | job        |                   |
   +----------------+     +------------+                   |
+->|id              |  +->|id          |                   |
|  |job_id          |o-+  |name        |                   |
+-o|up_id           |     |role_id     |o------------------+
   |                |     |description |
   +----------------+	  +------------+

```

## 内容版本控制

主表

```
CREATE TABLE `article` (
	`article_id` MEDIUMINT(8) UNSIGNED NOT NULL AUTO_INCREMENT,
	`cat_id` SMALLINT(5) NOT NULL DEFAULT '0',
	`title` VARCHAR(150) NOT NULL DEFAULT '',
	`content` LONGTEXT NOT NULL,
	`author` VARCHAR(30) NOT NULL DEFAULT '',
	`keywords` VARCHAR(255) NOT NULL DEFAULT '',
	PRIMARY KEY (`article_id`),
	INDEX `cat_id` (`cat_id`)
)
ENGINE=MyISAM
ROW_FORMAT=DEFAULT
AUTO_INCREMENT=1

```

本版控制表，用于记录每次变动

```
CREATE TABLE `article_history` (
	`id` MEDIUMINT(8) UNSIGNED NOT NULL AUTO_INCREMENT,
	`article_id` MEDIUMINT(8) UNSIGNED NOT NULL,
	`cat_id` SMALLINT(5) NOT NULL DEFAULT '0',
	`title` VARCHAR(150) NOT NULL DEFAULT '',
	`content` LONGTEXT NOT NULL,
	`author` VARCHAR(30) NOT NULL DEFAULT '',
	`keywords` VARCHAR(255) NOT NULL DEFAULT '',
	PRIMARY KEY (`id`),
	INDEX `article_id` (`article_id`)
)
ENGINE=MyISAM
ROW_FORMAT=DEFAULT
AUTO_INCREMENT=1

```

版本控制触发器

```
DROP TRIGGER article_history;

DELIMITER //
CREATE TRIGGER article_history BEFORE update ON article FOR EACH ROW
BEGIN
	INSERT INTO article_history SELECT * FROM article WHERE article_id = OLD.article_id;
END; //
DELIMITER;

```

## logging 日志表的设计

```
CREATE TABLE `logging` (
	`id` INT(10) UNSIGNED NOT NULL AUTO_INCREMENT,
	`tag` ENUM('unknow','www','user','admin') NOT NULL DEFAULT 'unknow' COMMENT '日志标签',
	`time` TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '产生时间',
	`facility` ENUM('bank','unionpay','sms','email') NOT NULL COMMENT '类别',
	`priority` ENUM('info','warning','error','critical','exception','debug') NOT NULL COMMENT '级别',
	`message` VARCHAR(512) NOT NULL COMMENT '内容',
	PRIMARY KEY (`id`)
)
COMMENT='日志表'
COLLATE='utf8_general_ci'
ENGINE=InnoDB
AUTO_INCREMENT=2;

```

分区日志表

```

delimiter $$

CREATE TABLE `logging` (
  `tag` enum('unknow','login','info','admin','cron','manual') NOT NULL DEFAULT 'unknow' COMMENT '日志标签',
  `asctime` datetime NOT NULL COMMENT '产生时间',
  `facility` enum('account','bank','unionpay','sms','email','unknow') NOT NULL DEFAULT 'unknow' COMMENT '类别',
  `priority` enum('info','warning','error','critical','exception','debug') NOT NULL DEFAULT 'debug' COMMENT '级别',
  `message` varchar(512) NOT NULL COMMENT '内容',
  `operator` varchar(50) NOT NULL DEFAULT 'computer' COMMENT '操作者'
) ENGINE=InnoDB DEFAULT CHARSET=utf8
/*!50100 PARTITION BY RANGE (YEAR(asctime))
SUBPARTITION BY HASH (MONTH(asctime))
(PARTITION p0 VALUES LESS THAN (1990) ENGINE = InnoDB,
 PARTITION p1 VALUES LESS THAN (2000) ENGINE = InnoDB,
 PARTITION p2 VALUES LESS THAN MAXVALUE ENGINE = InnoDB) */$$

```

分表+分区，每年分表一次，每个分区中保存一个月的数据

```
delimiter $$

CREATE TABLE `logging_2013` (
  `tag` enum('unknow','login','info','admin','cron','manual') NOT NULL DEFAULT 'unknow' COMMENT '日志标签',
  `asctime` datetime NOT NULL COMMENT '产生时间',
  `facility` enum('account','bank','unionpay','sms','email','unknow') NOT NULL DEFAULT 'unknow' COMMENT '类别',
  `priority` enum('info','warning','error','critical','exception','debug') NOT NULL DEFAULT 'debug' COMMENT '级别',
  `message` varchar(512) NOT NULL COMMENT '内容',
  `operator` varchar(50) NOT NULL DEFAULT 'computer' COMMENT '操作者'
) ENGINE=InnoDB DEFAULT CHARSET=utf8
/*!50100 PARTITION BY LIST (MONTH(asctime))
SUBPARTITION BY KEY (facility)
(PARTITION part0 VALUES IN (1) ENGINE = InnoDB,
 PARTITION part1 VALUES IN (2) ENGINE = InnoDB,
 PARTITION part2 VALUES IN (3) ENGINE = InnoDB,
 PARTITION part3 VALUES IN (4) ENGINE = InnoDB,
 PARTITION part4 VALUES IN (5) ENGINE = InnoDB,
 PARTITION part5 VALUES IN (6) ENGINE = InnoDB,
 PARTITION part6 VALUES IN (7) ENGINE = InnoDB,
 PARTITION part7 VALUES IN (8) ENGINE = InnoDB,
 PARTITION part8 VALUES IN (9) ENGINE = InnoDB,
 PARTITION part9 VALUES IN (10) ENGINE = InnoDB,
 PARTITION part10 VALUES IN (11) ENGINE = InnoDB,
 PARTITION part11 VALUES IN (12) ENGINE = InnoDB) */$$

```

命名分区

```
delimiter $$

CREATE TABLE `logging_2012` (
  `tag` enum('unknow','login','info','admin','cron','manual') NOT NULL DEFAULT 'unknow' COMMENT '日志标签',
  `asctime` datetime NOT NULL COMMENT '产生时间',
  `facility` enum('account','bank','unionpay','sms','email','unknow') NOT NULL DEFAULT 'unknow' COMMENT '类别',
  `priority` enum('info','warning','error','critical','exception','debug') NOT NULL DEFAULT 'debug' COMMENT '级别',
  `message` varchar(512) NOT NULL COMMENT '内容',
  `operator` varchar(50) NOT NULL DEFAULT 'computer' COMMENT '操作者'
) ENGINE=InnoDB DEFAULT CHARSET=utf8
/*!50100 PARTITION BY LIST (MONTH(asctime))
SUBPARTITION BY KEY (facility)
(PARTITION January VALUES IN (1) ENGINE = InnoDB,
 PARTITION February VALUES IN (2) ENGINE = InnoDB,
 PARTITION March VALUES IN (3) ENGINE = InnoDB,
 PARTITION April VALUES IN (4) ENGINE = InnoDB,
 PARTITION May VALUES IN (5) ENGINE = InnoDB,
 PARTITION June VALUES IN (6) ENGINE = InnoDB,
 PARTITION July VALUES IN (7) ENGINE = InnoDB,
 PARTITION August VALUES IN (8) ENGINE = InnoDB,
 PARTITION September VALUES IN (9) ENGINE = InnoDB,
 PARTITION October VALUES IN (10) ENGINE = InnoDB,
 PARTITION November VALUES IN (11) ENGINE = InnoDB,
 PARTITION December VALUES IN (12) ENGINE = InnoDB) */$$

```

## uuid 替代传统序列 id

```

DROP TABLE IF EXISTS `uuid_test`;
CREATE TABLE IF NOT EXISTS `uuid_test` (
  `uuid` varchar(36) NOT NULL,
  `name` varchar(20) NOT NULL,
  PRIMARY KEY (`uuid`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='uuid 测试';

```

插入触发器

```

DROP TRIGGER IF EXISTS `uuid_test_insert`;
SET @OLDTMP_SQL_MODE=@@SQL_MODE, SQL_MODE='';
DELIMITER //
CREATE TRIGGER `uuid_test_insert` BEFORE INSERT ON `uuid_test` FOR EACH ROW BEGIN
	IF new.uuid is null or new.uuid = '' or length(new.uuid) != 36 THEN
		set new.uuid=uuid();
	END IF;
END//
DELIMITER ;
SET SQL_MODE=@OLDTMP_SQL_MODE;

```

ID 放撰改触发器

```

DROP TRIGGER IF EXISTS `uuid_test_update`;
SET @OLDTMP_SQL_MODE=@@SQL_MODE, SQL_MODE='';
DELIMITER //
CREATE TRIGGER `uuid_test_update` BEFORE UPDATE ON `uuid_test` FOR EACH ROW BEGIN
	set new.uuid = old.uuid;
END//
DELIMITER ;
SET SQL_MODE=@OLDTMP_SQL_MODE;

```

## 动态配置表

很多时候我们需要使用数据库存储配置项，由于各种原因我们无法使用配置文件来完成，例如在一个有很多节点集群环境中使用文件配置文件时非常不方便。

```

DROP TABLE IF EXISTS `config`;
CREATE TABLE IF NOT EXISTS `config` (
  `key` varchar(50) NOT NULL,
  `value` varchar(50) NOT NULL,
  `operator` varchar(50) NOT NULL DEFAULT 'dba',
  `mtime` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`key`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='网站动态配置文件';

INSERT INTO `config` (`key`, `value`, `operator`, `mtime`) VALUES
	('cache.apc', 'ON', 'dba', '2013-07-18 16:17:13'),
	('cache.file.path', '/tmp', 'dba', '2013-07-18 16:17:57'),
	('cache.redis', 'YES', 'dba', '2013-07-18 16:17:22'),
	('payment.alipay.status', 'Enabled', 'dba', '2013-07-18 16:15:15'),
	('payment.alipay.url', 'http://xx.comx.com', 'dba', '2013-07-18 16:16:38'),
	('payment.yeepay.status', 'Enabled', 'dba', '2013-07-18 16:15:17'),
	('payment.99bill.status', 'Enabled', 'dba', '2013-07-18 16:15:10'),
	('payment.zqpay.status', 'Disabled', 'dba', '2013-07-18 16:15:20');

```

配置项 key 的写法很讲究

```
单个配置
database.host=localhost
database.user=user
database.pass=pass

多个配置
database.1.host=localhost
database.1.user=user
database.1.pass=pass
database.1.status=1

database.2.host=localhost
database.2.user=user
database.2.pass=pass
database.2.status=1

优化配置项，例如：payment.alipay.status 可以这样优化
payment.status.alipay
payment.status.yeepay

```

这样做的目的是为了更好的使用 like 进行查询

```
select `key`,`value` from config where `key` like 'payment.status.%';
select `key`,`value` from config where `key` like 'database.?.status';

```

### 配置表历史记录

我有一个表，里面只有固定行数的行记录，这些数据就是配置参数，我们将配置文件保存在数据库中，因为需要做负载均衡而不能使用文件配置文件。

有这样一个需求，这个记录每次修改都要保存历史记录，用于审计等等。我是这样设计该表的

```
CREATE TABLE `config_fee` (
	`id` INT(10) UNSIGNED NOT NULL AUTO_INCREMENT,
	`level` INT(11) NULL DEFAULT NULL COMMENT '层级',
	`type` ENUM('Deposit','Withdrawing') NOT NULL DEFAULT 'Withdrawing' COMMENT '类型，存款，取款',
	`min_fee` FLOAT(10,2) NOT NULL COMMENT '最低手续费',
	`max_fee` FLOAT(10,2) NOT NULL COMMENT '最高手续费',
	`ratio` FLOAT(10,2) NOT NULL COMMENT '手续费比例',
	`operator` VARCHAR(10) NOT NULL COMMENT '操作者',
	`status` ENUM('Current','Trash') NOT NULL DEFAULT 'Current',
	`ctime` TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
	`mtime` TIMESTAMP NULL DEFAULT NULL ON UPDATE CURRENT_TIMESTAMP,
	PRIMARY KEY (`id`)
)
COMMENT='手续费管理'
COLLATE='utf8_general_ci'
ENGINE=InnoDB;

```

数据记录的形态

```

mysql> select type,operator,status,ctime,mtime from config_mtf_fee;
+---------+----------+---------+---------------------+---------------------+
| type    | operator | status  | ctime               | mtime               |
+---------+----------+---------+---------------------+---------------------+
| Deposit | 141      | Trash   | 2014-08-28 11:10:17 | 2014-08-28 11:10:57 |
| Deposit | 141      | Trash   | 2014-08-28 11:10:17 | 2014-08-28 11:10:57 |
| Deposit | 141      | Trash   | 2014-08-28 11:10:17 | 2014-08-28 11:10:57 |
| Deposit | 141      | Trash   | 2014-08-28 11:10:17 | 2014-08-28 11:10:57 |
| Deposit | 324      | Current | 2014-08-28 11:10:54 | 2014-08-28 11:10:59 |
+---------+----------+---------+---------------------+---------------------+
2 rows in set (0.00 sec)

```

如上图所示，状态 Current 是当前记录，而 Trash 是废弃的历史记录。

每次修改数据，首先将 Current 改为 Trash，然后插入一条新数据状态为 Current，我们只会使用最后一条状态为 current 的数据。

## 验证码

用户注册，登陆等等需要验证码，下面的方案是，请求验证码生成一个随机验证码，存在 code 中，identity 可以存储来源 IP/手机号码/Cookie 等等用户校验， 5 分钟内如果没有被使用就会删除，如果 5 分钟内被使用也会删除。type 可以存放 www,user,bbs,admin.....等等，将所有验证码放在 captcha 表中统一管理。

这个方案主要是考虑没有 memcache/redis/apc/xcache 等等缓存环境下的解决方案，你也可以将下表改造一下，增加 ttl 字段用于存放生存时间，而不是采用 5 分钟一刀切的方案。

```
CREATE TABLE `captcha` (
	`id` INT(10) UNSIGNED NOT NULL AUTO_INCREMENT,
	`type` ENUM('user','admin') NOT NULL DEFAULT 'admin' COMMENT '验证码类型',
	`identity` VARCHAR(32) NOT NULL COMMENT '唯一身份识别 md5 摘要',
	`code` VARCHAR(6) NOT NULL COMMENT '验证码',
	`ctime` TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '创建时间',
	PRIMARY KEY (`id`),
	UNIQUE INDEX `code` (`code`),
	UNIQUE INDEX `identity` (`identity`)
)
COMMENT='验证码'
COLLATE='utf8_general_ci'
ENGINE=InnoDB;

```

```

CREATE EVENT `captcha` ON SCHEDULE
		EVERY 5 MINUTE STARTS '2013-07-08 16:27:03'
	ON COMPLETION PRESERVE
	ENABLE
	COMMENT ''
	DO BEGIN
	delete from captcha where type='myid' and ctime < DATE_ADD(now(), INTERVAL -5 MINUTE);
END

```

## 手机归属地数据库表

members_location 表与 members 表是一对一关系，该表只负责存储归属地信息

```
DROP TABLE IF EXISTS `members_location`;
CREATE TABLE IF NOT EXISTS `members_location` (
  `id` int(10) unsigned NOT NULL,
  `province` varchar(50) NOT NULL,
  `city` varchar(50) NOT NULL,
  `ctime` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`),
  KEY `province` (`province`),
  KEY `city` (`city`),
  CONSTRAINT `FK_members_location_members` FOREIGN KEY (`id`) REFERENCES `members` (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

```

当某些用户符合条件需要查询归属地是，只要将其插入到 members_mobile 表即可。该表使用黑洞引擎并不会存储手机号码，所以明文手机号码安全得到了保障。

```
DROP TABLE IF EXISTS `members_mobile`;
CREATE TABLE IF NOT EXISTS `members_mobile` (
  `id` int(10) NOT NULL,
  `number` varchar(11) NOT NULL
) ENGINE=BLACKHOLE DEFAULT CHARSET=utf8;

```

当有数据进入到 members_mobile 时出发器 members_mobile_insert 会工作，去 mobile_location 表中查询归属地后保存在 members_location 表中

```
DROP TRIGGER IF EXISTS `members_mobile_insert`;
SET @OLDTMP_SQL_MODE=@@SQL_MODE, SQL_MODE='';
DELIMITER //
CREATE TRIGGER `members_mobile_insert` BEFORE INSERT ON `members_mobile` FOR EACH ROW BEGIN
	insert into members_location(id,province,city) select NEW.id,mobile_location.province,mobile_location.city from  mobile_location where mobile_location.id = md5(LEFT(NEW.number, 7));
END//
DELIMITER ;
SET SQL_MODE=@OLDTMP_SQL_MODE;

```

mobile_location 是存储手机号段与归属地信息的数据库

```
DROP TABLE IF EXISTS `mobile_location`;
CREATE TABLE IF NOT EXISTS `mobile_location` (
  `id` varchar(50) NOT NULL,
  `province` varchar(50) DEFAULT NULL,
  `city` varchar(50) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

```

## 数据检查

### 身份证校验

该函数能够检查身份证号码是否正确

```

CREATE DEFINER=`neo`@`%` FUNCTION `check_id_number`(`idnumber` CHAR(18))
	RETURNS enum('true','false')
	LANGUAGE SQL
	NOT DETERMINISTIC
	NO SQL
	SQL SECURITY DEFINER
	COMMENT ''
BEGIN
DECLARE status ENUM('true','false') default 'false';
DECLARE verify CHAR(1);
DECLARE sigma INT;
DECLARE remainder INT;

IF length(idnumber) = 18 THEN
	set sigma = cast(substring(idnumber,1,1) as UNSIGNED) * 7
		+cast(substring(idnumber,2,1) as UNSIGNED) * 9
		+cast(substring(idnumber,3,1) as UNSIGNED) * 10
		+cast(substring(idnumber,4,1) as UNSIGNED) * 5
		+cast(substring(idnumber,5,1) as UNSIGNED) * 8
		+cast(substring(idnumber,6,1) as UNSIGNED) * 4
		+cast(substring(idnumber,7,1) as UNSIGNED) * 2
		+cast(substring(idnumber,8,1) as UNSIGNED) * 1
		+cast(substring(idnumber,9,1) as UNSIGNED) * 6
		+cast(substring(idnumber,10,1) as UNSIGNED) * 3
		+cast(substring(idnumber,11,1) as UNSIGNED) * 7
		+cast(substring(idnumber,12,1) as UNSIGNED) * 9
		+cast(substring(idnumber,13,1) as UNSIGNED) * 10
		+cast(substring(idnumber,14,1) as UNSIGNED) * 5
		+cast(substring(idnumber,15,1) as UNSIGNED) * 8
		+cast(substring(idnumber,16,1) as UNSIGNED) * 4
		+cast(substring(idnumber,17,1) as UNSIGNED) * 2;
	set remainder = MOD(sigma,11);
	set verify = (case remainder
		when 0 then '1' when 1 then '0' when 2 then 'X' when 3 then '9'
		when 4 then '8' when 5 then '7' when 6 then '6' when 7 then '5'
		when 8 then '4' when 9 then '3' when 10 then '2' else '/' end
	);

END IF;

IF right(idnumber,1) = verify THEN
	set status = 'true';
END IF;

RETURN status;

END

```

首先我们使用正确身份证号码进行测试，返回 true

```

mysql> select check_id_number('330702198003090915');
+---------------------------------------+
| check_id_number('330702198003090915') |
+---------------------------------------+
| true                                  |
+---------------------------------------+
1 row in set (0.01 sec)

```

长度不符合 18 位直接返回 false.

```

mysql> select check_id_number('33070219800309');
+-----------------------------------+
| check_id_number('33070219800309') |
+-----------------------------------+
| false                             |
+-----------------------------------+
1 row in set (0.00 sec)	

mysql> select check_id_number('33070219800309091457889');
+--------------------------------------------+
| check_id_number('33070219800309091457889') |
+--------------------------------------------+
| false                                      |
+--------------------------------------------+
1 row in set, 1 warning (0.00 sec)

```

随便改译为数，校验失败返回 false

```

mysql> select check_id_number('330702198003090914');
+---------------------------------------+
| check_id_number('330702198003090914') |
+---------------------------------------+
| false                                 |
+---------------------------------------+
1 row in set (0.00 sec)					

```

## 创建与修改时间

ctime 为创建时间, mtime 是修改时间默认 CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP

```
CREATE TABLE `trades_platform` (
	`id` INT(10) UNSIGNED NULL DEFAULT NULL,
	`type` ENUM('A','B','C') NOT NULL,
	`login` VARCHAR(10) NOT NULL,
	`password` VARCHAR(10) NOT NULL,
	`ctime` TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
	`mtime` TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
)
COMMENT='平台'
ENGINE=InnoDB;

```

我们还可以让 mtime 默认为 NULL,这样更节省存储空间。

```
	`ctime` TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
	`mtime` TIMESTAMP NULL DEFAULT NULL ON UPDATE CURRENT_TIMESTAMP,		

```

## 在线用户表

该表的功能是显示在线用户，控制多点登陆，防止异常退出

```

CREATE TABLE IF NOT EXISTS `employees_online` (
  `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
  `username` varchar(20) NOT NULL COMMENT 'User ID',
  `ipaddr` varchar(15) NOT NULL COMMENT 'IP Address',
  `expire` datetime NOT NULL COMMENT 'Expire time',
  `status` enum('Login','Logout','Offline') NOT NULL DEFAULT 'Login' COMMENT 'Current Status',
  `message` varchar(255) NOT NULL COMMENT 'Leave a message',
  `ctime` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT 'Created Time',
  `mtime` timestamp NULL DEFAULT NULL ON UPDATE CURRENT_TIMESTAMP COMMENT 'Modified Time',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='show listing of last logged in users';

DELIMITER //
CREATE DEFINER=`dba`@`192.168.%` EVENT `employees_online` ON SCHEDULE EVERY 15 MINUTE STARTS '2014-08-22 10:33:24' ON COMPLETION NOT PRESERVE ENABLE COMMENT 'Employees Online Logging' DO BEGIN
	update employees_online set `status` = 'Offline' where expire < now() and ctime > DATE_ADD(now(), INTERVAL -15 MINUTE);
END//
DELIMITER ;

SET @OLDTMP_SQL_MODE=@@SQL_MODE, SQL_MODE='NO_ENGINE_SUBSTITUTION';
DELIMITER //
CREATE TRIGGER `employees_online_before_delete` BEFORE DELETE ON `employees_online` FOR EACH ROW BEGIN
	SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Permission denied', MYSQL_ERRNO = 1001;
END//
DELIMITER ;
SET SQL_MODE=@OLDTMP_SQL_MODE;

SET @OLDTMP_SQL_MODE=@@SQL_MODE, SQL_MODE='NO_ENGINE_SUBSTITUTION';
DELIMITER //
CREATE TRIGGER `employees_online_before_update` BEFORE UPDATE ON `employees_online` FOR EACH ROW BEGIN
	SET NEW.`id` = OLD.id;
	SET NEW.`username` = OLD.username;
	SET NEW.`ipaddr` = OLD.ipaddr;
	SET NEW.`message` = OLD.message;
	SET NEW.`ctime` = OLD.ctime;
END//
DELIMITER ;
SET SQL_MODE=@OLDTMP_SQL_MODE;	

```

登陆日志将永久保存，防止数据被删除由触发器 employees_online_before_delete 负责

防止有人撰改登陆信息，由触发器 employees_online_before_update 负责，主要是防止撰改登陆名与 IP 地址，这样讲不能从其他电脑登陆，必须用户 Logout 才能在其他电脑登陆。

## HTML TO Text

实现 PHP strip_tags 函数的功能。

```

CREATE DEFINER=`dba`@`%` FUNCTION `strip_tags`(`$str` TEXT)
RETURNS text CHARSET utf8
LANGUAGE SQL
NOT DETERMINISTIC
CONTAINS SQL
SQL SECURITY DEFINER
COMMENT ''
BEGIN
    DECLARE $start, $end INT DEFAULT 1;
    LOOP
        SET $start = LOCATE("<", $str, $start);
        IF (!$start) THEN RETURN $str; END IF;
        SET $end = LOCATE(">", $str, $start);
        IF (!$end) THEN SET $end = $start; END IF;
        SET $str = INSERT($str, $start, $end - $start + 1, "");
    END LOOP;
END		

```

```

mysql> select strip_tags('<span><i>hello</i> <b>world</b>!!! <br /><a href="//www.netkiller/">netkiller</a>');
+----------------------------------------------------------------------+
| strip_tags('<span>hel<b>lo <a href="world">wo<>rld</a> <<x>again<.') |
+----------------------------------------------------------------------+
| hello world again.                                                   |
+----------------------------------------------------------------------+
1 row in set

mysql> select strip_tags('<span style="color:red"><i>hello</i> <b id="world" >world</b>!!! <br /><a class="home" href="//www.netkiller/">netkiller</a><span>') as TEXT;
+--------------------------+
| TEXT                     |
+--------------------------+
| hello world!!! netkiller |
+--------------------------+
1 row in set (0.00 sec)

```

## SNS 数据库设计

这里讲解 SNS 交友社区的数据库设计与实现

我们要实现下面几个功能

1.  朋友之间的关系，多对多关系
2.  朋友之间的维度，如 3 度 4 度....
3.  朋友的查找

```

CREATE DATABASE `sns` /*!40100 COLLATE 'utf8_general_ci' */

```

### people 表

people 是存储人，你可以用为 user,member 都可以

```

CREATE TABLE `people` (
	`id` INT(10) UNSIGNED NOT NULL AUTO_INCREMENT,
	`name` VARCHAR(50) NOT NULL,
	PRIMARY KEY (`id`)
)
COMMENT='Social Network Site - Six Degrees of Separation - http://www.netkiller.cn'
COLLATE='utf8_general_ci'
ENGINE=InnoDB;

```

存储具体的这人

### firend 表

这个表的功能主要是维持朋友之间的关系网，这里使用了多对多方式并且使用外键防止产生脏数据。

```

CREATE TABLE `friend` (
	`id` INT(10) UNSIGNED NOT NULL AUTO_INCREMENT,
	`people_id` INT(10) UNSIGNED NOT NULL,
	`friend_id` INT(10) UNSIGNED NOT NULL,
	`ctime` TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
	PRIMARY KEY (`id`),
	UNIQUE INDEX `unique` (`people_id`, `friend_id`),
	INDEX `FK_firend_people` (`people_id`),
	INDEX `FK_firend_people_2` (`friend_id`),
	CONSTRAINT `FK_firend_people` FOREIGN KEY (`people_id`) REFERENCES `people` (`id`),
	CONSTRAINT `FK_firend_people_2` FOREIGN KEY (`friend_id`) REFERENCES `people` (`id`)
)
COMMENT='Social Network Site - Six Degrees of Separation - http://www.netkiller.cn'
COLLATE='utf8_general_ci'
ENGINE=InnoDB;

```

### 演示

首先初始化用户数据

```

INSERT INTO `people` (`id`, `name`) VALUES
	(1, 'Neo'),
	(2, 'Luke'),
	(3, 'Jack'),
	(4, 'Joey'),
	(5, 'Jam'),
	(6, 'John');

```

建立朋友之间的关系

```

INSERT INTO `friend` (`id`, `people_id`, `friend_id`) VALUES
	(1, 1, 2),
	(2, 1, 3),
	(3, 1, 4),
	(4, 1, 5),
	(5, 1, 6),
	(6, 2, 1),
	(7, 2, 3);

```

现在就可以查找你的朋友了

```

select people.* from friend, people where friend.people_id = 1 and friend.friend_id = people.id;

```

查找朋友的朋友就比较麻烦了，必须使用递归方法，一层一层查下去，反复执行 SQL 效率是很低的，所以我们准备了第三张表。

### network 表

关系网表，主要功能是弥补 firend 表，用于快速检索（在不使用递归的情况下）

```

CREATE TABLE `network` (
	`id` INT(10) UNSIGNED NOT NULL AUTO_INCREMENT,
	`people_id` INT(10) UNSIGNED NOT NULL,
	`following_id` INT(10) UNSIGNED NOT NULL,
	`friend_id` INT(10) UNSIGNED NULL DEFAULT NULL,
	`degrees` VARCHAR(250) NOT NULL,
	`ctime` TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
	PRIMARY KEY (`id`),
	UNIQUE INDEX `unique` (`people_id`, `friend_id`, `following_id`),
	INDEX `FK_firend_people` (`people_id`),
	INDEX `FK_firend_people_2` (`friend_id`),
	INDEX `FK_friend_people_following_id` (`following_id`),
	CONSTRAINT `FK_firend_people` FOREIGN KEY (`people_id`) REFERENCES `people` (`id`),
	CONSTRAINT `FK_friend_people_following_id` FOREIGN KEY (`following_id`) REFERENCES `people` (`id`),
	CONSTRAINT `FK_friend_people_friend_id` FOREIGN KEY (`friend_id`) REFERENCES `people` (`id`)
)
COMMENT='Social Network Site - Six Degrees of Separation - http://www.netkiller.cn'
COLLATE='utf8_general_ci'
ENGINE=InnoDB;

```

following 一个朋友, Neo following Jam

```

INSERT INTO `people` (`id`, `name`) VALUES
	(1, 'Neo'),
	(2, 'Luke'),
	(3, 'Jack'),
	(4, 'Joey'),
	(5, 'Jam'),
	(6, 'John');

INSERT INTO `network` (`people_id`, `following_id`, `friend_id`, `degrees`) VALUES ( 1, 5, NULL, '1.5');

```

之前 Neo 已经 following Jam，接下来查找 Jam 的朋友，现在 Neo following John, John 是 Jam 的朋友，friend_id = NULL 表示 Jam 尚未有朋友

```

select * from network where people_id=1 and friend_id = 5;

INSERT INTO `sns`.`network` (`people_id`, `following_id`, `friend_id`, `degrees`) VALUES ('1', '6', '5', '1.5.6');

```

Neo following Joey, Joey 是 Luke 的朋友， 所以 Luke 可能是 Neo 的朋友

```

INSERT INTO `sns`.`network` (`people_id`, `following_id`, `friend_id`, `degrees`) VALUES ('1', '4', '2', '1.2.4');

```

查询不同维度下的所有好友，查询出的用户 ID 需要处理。

```

select * from network where people_id=1 and degrees like "1.%";
select * from network where people_id=1 and degrees like "1.2%";
select * from network where people_id=1 and degrees like "1.2.%";

```

至此社区管理网就建立起来了

上面的例子演示了 people_id=1 即 Neo 的关系网

## PostgreSQL 所特有数据库设计

### PostgreSQL 数据库 ORDBMS / OODBMS 等特有属性

对象相关数据库管理系统(ORDBMS Object – Oriented Relative DBMS)

### 国家地区表的设计

```

 +-----------+
 | city      |
 |-----------|
 |id         | <---+
 |name       |     |
 |description|    1:n
 |status     |     |
 |parent_id  | o---+
 +-----------+

```

例 4.2. 递归查询实例 city 表

定义结构

```

CREATE TABLE city
(
  id serial NOT NULL,
  name character varying,
  parent_id integer,
  status boolean,
  CONSTRAINT city_pkey PRIMARY KEY (id),
  CONSTRAINT city_parent_id_fkey FOREIGN KEY (parent_id)
      REFERENCES city (id) MATCH SIMPLE
      ON UPDATE NO ACTION ON DELETE NO ACTION
)
WITH (
  OIDS=FALSE
);
ALTER TABLE city
  OWNER TO sys;

```

插入数据

```

INSERT INTO city (id, name, parent_id, status) VALUES (1, '广东', NULL, NULL);
INSERT INTO city (id, name, parent_id, status) VALUES (2, '湖南', NULL, NULL);
INSERT INTO city (id, name, parent_id, status) VALUES (3, '深圳', 1, NULL);
INSERT INTO city (id, name, parent_id, status) VALUES (4, '东莞', 1, NULL);
INSERT INTO city (id, name, parent_id, status) VALUES (5, '福田', 3, NULL);
INSERT INTO city (id, name, parent_id, status) VALUES (6, '南山', 3, NULL);
INSERT INTO city (id, name, parent_id, status) VALUES (7, '宝安', 3, NULL);
INSERT INTO city (id, name, parent_id, status) VALUES (8, '西乡', 7, NULL);
INSERT INTO city (id, name, parent_id, status) VALUES (9, '福永', 7, NULL);
INSERT INTO city (id, name, parent_id, status) VALUES (10, '龙华', 7, NULL);
INSERT INTO city (id, name, parent_id, status) VALUES (11, '长沙', 2, NULL);
INSERT INTO city (id, name, parent_id, status) VALUES (12, '湘潭', 2, NULL);
INSERT INTO city (id, name, parent_id, status) VALUES (13, '常德', 2, NULL);
INSERT INTO city (id, name, parent_id, status) VALUES (14, '桃源', 13, NULL);
INSERT INTO city (id, name, parent_id, status) VALUES (15, '汉寿', 13, NULL);
INSERT INTO city (id, name, parent_id, status) VALUES (16, '黑龙江', NULL, NULL);
INSERT INTO city (id, name, parent_id, status) VALUES (17, '伊春', 16, NULL);
INSERT INTO city (id, name, parent_id, status) VALUES (18, '哈尔滨', 16, NULL);
INSERT INTO city (id, name, parent_id, status) VALUES (19, '齐齐哈尔', 16, NULL);
INSERT INTO city (id, name, parent_id, status) VALUES (20, '牡丹江', 16, NULL);
INSERT INTO city (id, name, parent_id, status) VALUES (21, '佳木斯', 16, NULL);
INSERT INTO city (id, name, parent_id, status) VALUES (22, '民治', 10, NULL);
INSERT INTO city (id, name, parent_id, status) VALUES (23, '上塘', 10, NULL);

```

查询

```

WITH RECURSIVE path(id, name, path, idpath, parent_id, status) AS (
  SELECT id, name, '/' || name , '/' || id , parent_id, status FROM city WHERE parent_id is null
  UNION
  SELECT
    city.id,
    city.name,
    parentpath.path ||
      CASE parentpath.path
	WHEN '/' THEN ''
	ELSE '/'
      END || city.name,
    parentpath.idpath ||
     CASE parentpath.idpath
	WHEN '/' THEN ''
	ELSE '/'
      END || city.id,
    city.parent_id, city.status
  FROM city, path as parentpath
  WHERE city.parent_id = parentpath.id
)

SELECT * FROM path;

```

结果输出

```

 id |   name   |           path            |    idpath    | parent_id | status
----+----------+---------------------------+--------------+-----------+--------
  1 | 广东     | /广东                     | /1           |           |
  2 | 湖南     | /湖南                     | /2           |           |
 16 | 黑龙江   | /黑龙江                   | /16          |           |
  3 | 深圳     | /广东/深圳                | /1/3         |         1 |
  4 | 东莞     | /广东/东莞                | /1/4         |         1 |
 11 | 长沙     | /湖南/长沙                | /2/11        |         2 |
 12 | 湘潭     | /湖南/湘潭                | /2/12        |         2 |
 13 | 常德     | /湖南/常德                | /2/13        |         2 |
 17 | 伊春     | /黑龙江/伊春              | /16/17       |        16 |
 18 | 哈尔滨   | /黑龙江/哈尔滨            | /16/18       |        16 |
 19 | 齐齐哈尔 | /黑龙江/齐齐哈尔          | /16/19       |        16 |
 20 | 牡丹江   | /黑龙江/牡丹江            | /16/20       |        16 |
 21 | 佳木斯   | /黑龙江/佳木斯            | /16/21       |        16 |
  5 | 福田     | /广东/深圳/福田           | /1/3/5       |         3 |
  6 | 南山     | /广东/深圳/南山           | /1/3/6       |         3 |
  7 | 宝安     | /广东/深圳/宝安           | /1/3/7       |         3 |
 14 | 桃源     | /湖南/常德/桃源           | /2/13/14     |        13 |
 15 | 汉寿     | /湖南/常德/汉寿           | /2/13/15     |        13 |
  8 | 西乡     | /广东/深圳/宝安/西乡      | /1/3/7/8     |         7 |
  9 | 福永     | /广东/深圳/宝安/福永      | /1/3/7/9     |         7 |
 10 | 龙华     | /广东/深圳/宝安/龙华      | /1/3/7/10    |         7 |
 22 | 民治     | /广东/深圳/宝安/龙华/民治 | /1/3/7/10/22 |        10 |
 23 | 上塘     | /广东/深圳/宝安/龙华/上塘 | /1/3/7/10/23 |        10 |
(23 rows)

```

### 话题讨论表的设计

例 4.3. 话题讨论表的设计

http://justcramer.com/2010/05/30/scaling-threaded-comments-on-django-at-disqus/

```

create table comments (
    id SERIAL PRIMARY KEY,
    message VARCHAR,
    author VARCHAR,
    parent_id INTEGER REFERENCES comments(id)
);
insert into comments (message, author, parent_id)
    values ('This thread is really cool!', 'David', NULL), ('Ya David, we love it!', 'Jason', 1), ('I agree David!', 'Daniel', 1), ('gift Jason', 'Anton', 2),
    ('Very interesting post!', 'thedz', NULL), ('You sir, are wrong', 'Chris', 5), ('Agreed', 'G', 5), ('Fo sho, Yall', 'Mac', 5);

```

```

WITH RECURSIVE cte (id, message, author, path, parent_id, depth)  AS (
    SELECT  id,
        message,
        author,
        array[id] AS path,
        parent_id,
        1 AS depth
    FROM    comments
    WHERE   parent_id IS NULL

    UNION ALL

    SELECT  comments.id,
        comments.message,
        comments.author,
        cte.path || comments.id,
        comments.parent_id,
        cte.depth + 1 AS depth
    FROM    comments
    JOIN cte ON comments.parent_id = cte.id
    )
    SELECT id, message, author, path, depth FROM cte ORDER BY path;

```

输出结果

```
 id |           message           | author |  path   | depth
----+-----------------------------+--------+---------+-------
  1 | This thread is really cool! | David  | {1}     |     1
  2 | Ya David, we love it!       | Jason  | {1,2}   |     2
  4 | gift Jason                  | Anton  | {1,2,4} |     3
  3 | I agree David!              | Daniel | {1,3}   |     2
  5 | Very interesting post!      | thedz  | {5}     |     1
  6 | You sir, are wrong          | Chris  | {5,6}   |     2
  7 | Agreed                      | G      | {5,7}   |     2
  8 | Fo sho, Yall                | Mac    | {5,8}   |     2
(8 rows)

```

### 账户表/余额表/消费储蓄表

此表适用于购物车等金钱来往账面等等。

```

-- Table: account

-- DROP TABLE account;

CREATE TABLE account
(
  id integer NOT NULL DEFAULT nextval('trade_id_seq'::regclass),
  no character varying(10) NOT NULL, -- 账号
  balance money NOT NULL DEFAULT 0.00, -- 余额
  datetime timestamp without time zone NOT NULL DEFAULT (now())::timestamp(0) without time zone,
  CONSTRAINT account_pkey PRIMARY KEY (id)
)
WITH (
  OIDS=FALSE
);
ALTER TABLE account
  OWNER TO dba;
COMMENT ON COLUMN account.no IS '账号';
COMMENT ON COLUMN account.balance IS '余额';

-- Index: account_no_idx

-- DROP INDEX account_no_idx;

CREATE INDEX account_no_idx
  ON account
  USING btree
  (no COLLATE pg_catalog."default");

```

账户结余计算

```

select acc.*, (select sum(balance)+acc.balance from account as ac where ac.id < acc.id) as profit from account as acc;

test=# select acc.*, (select sum(balance)+acc.balance from account as ac where ac.id < acc.id) as profit from account as acc;
 id |  no  | balance  |      datetime       | profit
----+------+----------+---------------------+---------
  1 | 1000 |    $0.00 | 2013-10-09 10:51:10 |
  2 | 1000 |   $12.60 | 2013-10-09 10:51:22 |  $12.60
  4 | 1000 |   $16.80 | 2013-10-09 10:51:42 |  $29.40
  5 | 1000 |  $100.00 | 2013-10-09 10:51:49 | $129.40
  6 | 1000 |  $200.00 | 2013-10-09 10:56:35 | $329.40
  7 | 1000 |   $50.45 | 2013-10-09 10:57:23 | $379.85
  8 | 1000 |   $75.50 | 2013-10-09 10:57:31 | $455.35
  9 | 1000 |  -$55.30 | 2013-10-09 10:59:28 | $400.05
 10 | 1000 | -$200.00 | 2013-10-09 10:59:44 | $200.05
(9 rows)

```

## 第 5 章 数据库安全

## 保护表

保护表中的数据不被删除，当记录被用户删除时会提示"Permission denied" 权限拒绝

```

CREATE DEFINER=`root`@`192.168.%` TRIGGER `member_before_delete` BEFORE DELETE ON `member` FOR EACH ROW BEGIN
	SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Permission denied', MYSQL_ERRNO = 1001;
END		

```

## 保护表字段

通过触发器，使之无法修改某些字段的数据，同时不影响修改其他字段。

```
DROP TRIGGER IF EXISTS `members`;
SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='';
DELIMITER //
CREATE TRIGGER `members` BEFORE UPDATE ON `members` FOR EACH ROW BEGIN
	set new.name 		= old.name;
	set new.cellphone 	= old.cellphone;
	set new.email 		= old.email;
    set new.password 	= old.password;
END//
DELIMITER ;
SET SQL_MODE=@OLD_SQL_MODE;

```

再举一个例子

```
CREATE TABLE `account` (
	`id` INT(10) UNSIGNED NOT NULL AUTO_INCREMENT,
	`user` VARCHAR(50) NOT NULL DEFAULT '0',
	`cash` FLOAT NOT NULL DEFAULT '0',
	PRIMARY KEY (`id`)
)
COLLATE='utf8_general_ci'
ENGINE=InnoDB;

```

每一次数据变化新增一条数据

```
INSERT INTO `test`.`account` (`user`, `cash`) VALUES ('neo', -10);
INSERT INTO `test`.`account` (`user`, `cash`) VALUES ('neo', -5);
INSERT INTO `test`.`account` (`user`, `cash`) VALUES ('neo', 30);
INSERT INTO `test`.`account` (`user`, `cash`) VALUES ('neo', -20);

```

保护用户的余额不被修改

```
DROP TRIGGER IF EXISTS `account`;
SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='';
DELIMITER //
CREATE TRIGGER `account` BEFORE UPDATE ON `account` FOR EACH ROW BEGIN
	set new.cash 		= old.cash;
END//
DELIMITER ;
SET SQL_MODE=@OLD_SQL_MODE;

```

## 时间一致性

经常会因为每个服务器的时间不同，导致插入数据有问题，虽然可以采用 ntp 服务同步时间，但由于各种因素仍然会出问题，怎么解决？我建议以数据库时间为准。

MySQL 5.6 之前的版本

默认值为当前时间

```
CREATE TABLE `tdate` (
	`id` INT(11) NOT NULL AUTO_INCREMENT,
	`ctime` TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
	`mtime` TIMESTAMP NOT NULL DEFAULT '0000-00-00 00:00:00' COMMENT '修改时间',
	PRIMARY KEY (`id`)
)
COLLATE='utf8_general_ci'
ENGINE=InnoDB;

```

MySQL 不允许一个表拿有两个默认时间。我一无法兼顾修改时间，我们舍弃创建时间，当有数据变化 ON UPDATE CURRENT_TIMESTAMP 自动修改时间

```
CREATE TABLE `tdate` (
	`id` INT(11) NOT NULL AUTO_INCREMENT,
	`ctime` TIMESTAMP NOT NULL DEFAULT '0000-00-00 00:00:00' COMMENT '创建时间',
	`mtime` TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '修改时间',
	PRIMARY KEY (`id`)
)
COLLATE='utf8_general_ci'
ENGINE=InnoDB;

```

插入创建时间 insert into tdate(ctime) values(CURRENT_TIMESTAMP); 不要采用 insert into tdate(ctime) values('2013-12-02 08:20:06');这种方法，尽量让数据库处理时间。

MySQL 5.6 之后版本，可以实现创建时间为系统默认，修改时间创建的时候默认为空，当修改数据的时候更新时间。

```

CREATE TABLE `tdate` (
	`id` INT(11) NOT NULL AUTO_INCREMENT,
	`ctime` TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
	`mtime` TIMESTAMP NULL DEFAULT NULL ON UPDATE CURRENT_TIMESTAMP COMMENT '修改时间',
	PRIMARY KEY (`id`)
)
COLLATE='utf8_general_ci'
ENGINE=InnoDB;

```

## 为数据安全而分库

我们通常使用一个数据库开发，该数据库包含了前后台所有的功能，我建议将前后台等等功能进行分库然后对应各种平台分配用户权限，例如

我们创建三个数据库 cms,frontend,backend 同时对应创建三个用户 cms,frontend,backend 三个用户只能分别访问自己的数据库，注意在系统的设计之初你要考虑好这样的划分随之系统需要做相应的调整。

```
CREATE DATABASE `cms` /*!40100 COLLATE 'utf8_general_ci' */;
CREATE DATABASE `frontend` /*!40100 COLLATE 'utf8_general_ci' */;
CREATE DATABASE `backend` /*!40100 COLLATE 'utf8_general_ci' */;

```

backend 负责后台，权限最高

```
mysql> SHOW GRANTS FOR 'backend'@'localhost';
+--------------------------------------------------------------------------------------+
| Grants for backend@localhost                                                         |
+--------------------------------------------------------------------------------------+
| GRANT USAGE ON *.* TO 'backend'@'localhost'                                          |
| GRANT SELECT, INSERT, UPDATE, DELETE ON `cms`.* TO 'backend'@'localhost'             |
| GRANT SELECT, INSERT, UPDATE, DELETE ON `frontend`.* TO 'backend'@'localhost'        |
| GRANT SELECT, INSERT, UPDATE, DELETE, CREATE ON `backend`.* TO 'backend'@'localhost' |
+--------------------------------------------------------------------------------------+
4 rows in set (0.04 sec)		

```

frontend 是前台权限，主要是用户用户中心，用户注册，登录，用户信息资料编辑，查看新闻等等

```
mysql> SHOW GRANTS FOR 'frontend'@'localhost';
+------------------------------------------------------------------------+
| Grants for frontend@localhost                                          |
+------------------------------------------------------------------------+
| GRANT USAGE ON *.* TO 'frontend'@'localhost'                           |
| GRANT SELECT, INSERT, UPDATE ON `frontend`.* TO 'frontend'@'localhost' |
| GRANT SELECT ON `cms`.`news` TO 'frontend'@'localhost'                 |
+------------------------------------------------------------------------+
3 rows in set (0.00 sec)		

```

cms 用户是网站内容管理，主要负责内容更新，但登陆 CMS 后台需要`backend`.`Employees`表用户认证，所以他需要读取权限，但不允许修改其中的数据。

```
mysql> SHOW GRANTS FOR 'cms'@'localhost';
+----------------------------------------------------------------------+
| Grants for cms@localhost                                             |
+----------------------------------------------------------------------+
| GRANT USAGE ON *.* TO 'cms'@'localhost'                              |
| GRANT SELECT, INSERT, UPDATE, DELETE ON `cms`.* TO 'cms'@'localhost' |
| GRANT SELECT ON `backend`.`Employees` TO 'cms'@'localhost'           |
+----------------------------------------------------------------------+
3 rows in set (0.00 sec)		

```

## 内容版本控制,撰改留痕

主表

```
CREATE TABLE `article` (
	`article_id` MEDIUMINT(8) UNSIGNED NOT NULL AUTO_INCREMENT,
	`cat_id` SMALLINT(5) NOT NULL DEFAULT '0',
	`title` VARCHAR(150) NOT NULL DEFAULT '',
	`content` LONGTEXT NOT NULL,
	`author` VARCHAR(30) NOT NULL DEFAULT '',
	`keywords` VARCHAR(255) NOT NULL DEFAULT '',
	PRIMARY KEY (`article_id`),
	INDEX `cat_id` (`cat_id`)
)
ENGINE=MyISAM
ROW_FORMAT=DEFAULT
AUTO_INCREMENT=1

```

用于记录每次修改变动，通过该表，可以追朔数据库记录被什么时候修改过，修改了那些内容。

```
CREATE TABLE `article_history` (
	`id` MEDIUMINT(8) UNSIGNED NOT NULL AUTO_INCREMENT,
	`article_id` MEDIUMINT(8) UNSIGNED NOT NULL,
	`cat_id` SMALLINT(5) NOT NULL DEFAULT '0',
	`title` VARCHAR(150) NOT NULL DEFAULT '',
	`content` LONGTEXT NOT NULL,
	`author` VARCHAR(30) NOT NULL DEFAULT '',
	`keywords` VARCHAR(255) NOT NULL DEFAULT '',
	PRIMARY KEY (`id`),
	INDEX `article_id` (`article_id`)
)
ENGINE=MyISAM
ROW_FORMAT=DEFAULT
AUTO_INCREMENT=1

```

版本控制触发器

```
DROP TRIGGER article_history;

DELIMITER //
CREATE TRIGGER article_history BEFORE update ON article FOR EACH ROW
BEGIN
	INSERT INTO article_history SELECT * FROM article WHERE article_id = OLD.article_id;
END; //
DELIMITER;

```

进一步优化，我们可以为 history 历史表增加时间字段，用于记录被撰改那一时刻的时间。

```

CREATE TABLE `article_history` (
	`id` MEDIUMINT(8) UNSIGNED NOT NULL AUTO_INCREMENT,
	`article_id` MEDIUMINT(8) UNSIGNED NOT NULL,
	`cat_id` SMALLINT(5) NOT NULL DEFAULT '0',
	`title` VARCHAR(150) NOT NULL DEFAULT '',
	`content` LONGTEXT NOT NULL,
	`author` VARCHAR(30) NOT NULL DEFAULT '',
	`keywords` VARCHAR(255) NOT NULL DEFAULT '',
	`ctime` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT 'Created Time',
  	`mtime` timestamp NULL DEFAULT NULL ON UPDATE CURRENT_TIMESTAMP COMMENT 'Modified Time',
	PRIMARY KEY (`id`),
	INDEX `article_id` (`article_id`)
)
ENGINE=MyISAM
ROW_FORMAT=DEFAULT
AUTO_INCREMENT=1

```

我们还可以为该表（article_history）增加出发器，任何修改将被拒绝.

## 数据库审计表

与上一章节所提到的历史表不同，历史表需要经常翻查所以我们需要用到索引。审计表通常是数据归档，不允许修改，且基本上很少访问。

```

CREATE TABLE `order` (
  `id` int(10) unsigned NOT NULL AUTO_INCREMENT COMMENT '订单 ID',
  `name` varchar(45) NOT NULL COMMENT '订单名称',
  `price` float NOT NULL COMMENT '价格',
  `ctime` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='订单表'		

```

基于 order 表创建 order_audit 审计表

```

create table order_audit engine=archive as select * from `order`;

```

order_audit 表结构如下

```

CREATE TABLE `order_audit` (
  `id` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '订单 ID',
  `name` varchar(45) NOT NULL COMMENT '订单名称',
  `price` float NOT NULL COMMENT '价格',
  `ctime` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间'
) ENGINE=ARCHIVE DEFAULT CHARSET=utf8		

```

创建插入和更新触发器，用于插入和修改的时候同事写入一份到归档表中。

```

DROP TRIGGER IF EXISTS `test`.`order_AFTER_INSERT`;

DELIMITER $$
USE `test`$$
CREATE DEFINER=`dba`@`%` TRIGGER `test`.`order_AFTER_INSERT` AFTER INSERT ON `order` FOR EACH ROW
BEGIN
	INSERT INTO order_audit SELECT * FROM `order` WHERE id = NEW.id; 
END$$
DELIMITER ;
DROP TRIGGER IF EXISTS `test`.`order_AFTER_UPDATE`;

DELIMITER $$
USE `test`$$
CREATE DEFINER=`dba`@`%` TRIGGER `test`.`order_AFTER_UPDATE` AFTER UPDATE ON `order` FOR EACH ROW
BEGIN
	INSERT INTO order_audit SELECT * FROM `order` WHERE id = NEW.id; 
END$$
DELIMITER ;

```

## 用户/角色认证

本小节我们实现一个功能，当用户插入，修改或者删除数据时，判断该操作是否具备应有的权限。如果权限不符合就拒绝操作同时提示用户。

```

CREATE TABLE `staff` (
	`id` INT(10) UNSIGNED NOT NULL AUTO_INCREMENT COMMENT '员工 ID',
	`name` VARCHAR(50) NOT NULL COMMENT '员工名字',
	PRIMARY KEY (`id`)
)
COMMENT='员工表'
COLLATE='utf8_general_ci'
ENGINE=InnoDB;

INSERT INTO `staff` (`id`, `name`) VALUES
	(1, 'Neo'),
	(2, 'Luke'),
	(2, 'Jack');

```

staff 是员工表与下面的 staff_has_role 配合使用，形成员工与权限一对多关系。

```

CREATE TABLE `staff_has_role` (
	`id` INT(10) UNSIGNED NOT NULL AUTO_INCREMENT,
	`staff_id` INT(10) UNSIGNED NOT NULL COMMENT '员工 ID',
	`role` ENUM('Create','Update','Delete') NOT NULL COMMENT '角色',
	PRIMARY KEY (`id`),
	INDEX `FK_staff_has_role_staff` (`staff_id`),
	CONSTRAINT `FK_staff_has_role_staff` FOREIGN KEY (`staff_id`) REFERENCES `staff` (`id`) ON UPDATE CASCADE ON DELETE CASCADE
)
COLLATE='utf8_general_ci'
ENGINE=InnoDB;

INSERT INTO `staff_has_role` (`id`, `staff_id`, `role`) VALUES
	(1, 1, 'Create'),
	(2, 1, 'Delete'),
	(3, 1, 'Update'),
	(4, 2, 'Delete'),
	(5, 3, 'Create');
	(6, 3, 'Update');

```

权限表可以进一步优化，角色拥有组功能，实现颗粒度更细的权限控制，有情趣看前面的相关章节。

```

CREATE TABLE `product` (
	`id` INT(10) UNSIGNED NOT NULL AUTO_INCREMENT COMMENT '唯一 ID',
	`name` VARCHAR(10) NOT NULL COMMENT '名称',
	`sn` VARCHAR(10) NOT NULL COMMENT '序列号',
	`price` FLOAT NOT NULL COMMENT '价格',
	`amount` SMALLINT(6) NOT NULL COMMENT '数量',
	`staff_id` INT(10) UNSIGNED NOT NULL COMMENT '员工 ID',
	`ctime` TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
	`mtime` TIMESTAMP NULL DEFAULT NULL ON UPDATE CURRENT_TIMESTAMP COMMENT '修改时间',
	PRIMARY KEY (`id`),
	UNIQUE INDEX `sn` (`sn`),
	INDEX `FK_product_staff` (`staff_id`),
	CONSTRAINT `FK_product_staff` FOREIGN KEY (`staff_id`) REFERENCES `staff` (`id`)
)
COMMENT='产品表'
COLLATE='utf8_general_ci'
ENGINE=InnoDB;

```

以产品表为例，这里要实现的是对产品表记录的权限控制。例如 Neo 有用插入，修改和删除权限，Luke 的 Create 与 Update 权限被吊销，只能删除他之前创建的数据。而 Jack 只有能创建于更新数据。

下面的三个触发器完成具体的权限控制。同样你可以进一步优化下面的代码的权限颗粒度，使之能控制到具体列，甚至具体的记录。

```

CREATE DEFINER=`root`@`%` TRIGGER `product_before_delete` BEFORE DELETE ON `product` FOR EACH ROW BEGIN
	if not exists(select id from staff where id=OLD.staff_id and role="delete") then
		SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Permission denied', MYSQL_ERRNO = 1001;
	end if;
END

CREATE DEFINER=`root`@`%` TRIGGER `product_before_insert` BEFORE INSERT ON `product` FOR EACH ROW BEGIN
	 if not exists(select id from staff where id=NEW.staff_id and role="create") then
	       SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = "The staff's role is not correct or it does not exist.", MYSQL_ERRNO = 1001;
	 end if;
END

CREATE DEFINER=`root`@`%` TRIGGER `product_before_update` BEFORE UPDATE ON `product` FOR EACH ROW BEGIN
	if not exists(select id from staff where id=NEW.staff_id and role="update") then
		SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = "The staff's role cannot update data.", MYSQL_ERRNO = 1001;
	end if;
END

```

Neo 测试如下

```

INSERT INTO `test`.`product` (`name`, `sn`, `price`, `amount`, `staff_id`, `ctime`) VALUES ('Iphone', '678624', '5000', '77', '1', '2010-08-18 15:38:23');
SELECT LAST_INSERT_ID();

UPDATE `test`.`product` SET `name`='HTC', `sn`='5544467', `price`='2000' WHERE  `id`=2;

DELETE FROM `test`.`product` WHERE  `id`=1;

```

Luke 测试如下：

```

INSERT INTO `test`.`product` (`name`, `sn`, `price`, `amount`, `staff_id`) VALUES ('Nokia', '65722', '800', '55', '2');
/* SQL 错误（1001）：The staff's role is not correct or it does not exist. */

UPDATE `test`.`product` SET `name`='HTC', `sn`='5544467', `price`='2000', staff_id=2 WHERE  `id`=2;
/* SQL 错误（1001）：The staff's role cannot update data. */

```

## Token 认证

我们在 staff 表的基础上增加 token 字段

```

CREATE TABLE `staff` (
	`id` INT(10) UNSIGNED NOT NULL AUTO_INCREMENT COMMENT '员工 ID',
	`name` VARCHAR(50) NOT NULL COMMENT '员工名字',
	`token` VARCHAR(32) NOT NULL COMMENT 'Token 校验',
	PRIMARY KEY (`id`)
)
COMMENT='员工表'
COLLATE='utf8_general_ci'
ENGINE=InnoDB;		

```

插入数据的时候增加一些干扰字符串，这里使用 concat(NEW.id,'+',NEW.name,'-')

```

CREATE DEFINER=`root`@`%` TRIGGER `staff_before_insert` BEFORE INSERT ON `staff` FOR EACH ROW BEGIN

if md5(concat(NEW.id,'+',NEW.name,'-')) != NEW.token then
		SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Permission denied', MYSQL_ERRNO = 1001;
	end if;

END

```

注意表权限可以授权给用户，触发器权限不让普通用户查看。否则用户看到 concat(NEW.id,'+',NEW.name,'-') 就没有意义了。

下面开始测试：

```

INSERT INTO `test`.`staff` (`name`, `token`) VALUES ('John', '678797066');
/* SQL 错误（1001）：Permission denied */	

```

下面再测试，首先生成一个正确的 tokon, 然后使用该 token 插入数据：

```

-- 通过下面语句生成一个 Token
select md5(concat('5','+','Jam','-')) as token;

-- 使用上面的 Token 插入数据
INSERT INTO `test`.`staff` (`id`, `name`, `token`) VALUES (5, 'Jam', '1b033ce21cbadacabc9f0c38fb58dbb2');

SELECT * FROM `test`.`staff` WHERE `id` = 5;

```

开发注意事项, Token 生成算法要保密，不要使用下面 SQL 提交数据

```
INSERT INTO `test`.`staff` (`id`, `name`, `token`) VALUES (5, 'Jam', md5(concat('5','+','Jam','-')));		

```

应该分两步，一是计算 Token，二是插入数据。可以将 Token 计算交给程序而不是 SQL，并且封装在。jar(Java)中或者。so(PHP 扩展中).

## 数据加密

数据库中有很多敏感字段，不允许随意查看，例如开发人员，运维人员，甚至 DBA 数据库管理员。另外加密主要是防止被黑客脱库（盗走）

敏感数据加密有很多办法，可以用数据库内部加密函数，也可以在外部处理后写入数据库。加密算法有很多种，但通常两类比较常用，一种是通过 key 加密解密，另一种是通过证书加密解密。

通常程序员负责写程序，程序交给运维配置，运维将 key 设置好，运维不能有数据库权限，DBA 只能登陆数据库，没有 key 权限。

### AES_ENCRYPT / AES_DECRYPT

这里介绍 AES 加密与解密简单用法

```

mysql> select AES_ENCRYPT('helloworld','key');
+---------------------------------+
| AES_ENCRYPT('helloworld','key') |
+---------------------------------+
|                                 |
+---------------------------------+
1 row in set (0.00 sec)

mysql> select AES_DECRYPT(AES_ENCRYPT('helloworld','key'),'key');
+----------------------------------------------------+
| AES_DECRYPT(AES_ENCRYPT('helloworld','key'),'key') |
+----------------------------------------------------+
| helloworld                                         |
+----------------------------------------------------+
1 row in set (0.00 sec)

mysql>

```

### 加密字段

加密数据入库

```

CREATE TABLE `encryption` (
	`mobile` VARBINARY(16) NOT NULL,
	`key` VARCHAR(32) NOT NULL
)
ENGINE=InnoDB;

INSERT INTO encryption(`mobile`,`key`)VALUES( AES_ENCRYPT('13691851789',md5('13691851789')), md5('13691851789')) 
select AES_DECRYPT(mobile,`key`), length(mobile) from encryption;

```

这里方便演示将 key 写入了数据库，实际应用 key 应该存储在应用程序配置文件中。通常能把获得 key 的人不应该用数据库权限。

## 开发加密插件开发

数据库内部提供的摘要函数 MD5/SHA/CRC 与现有的 AES/DES 加密函数以及不能满足我们的需求，所以我们有必要开发外挂插件实现数据加密。

这里有一个例子，是我早年开发的 [`github.com/netkiller/mysql-safenet-plugin`](https://github.com/netkiller/mysql-safenet-plugin) 这个 UDF 是链接 Safenet 设备，实现数据库加密记录。

saftnet.h

```

my_bool safenet_encrypt_init(UDF_INIT *initid, UDF_ARGS *args, char *message);
char *safenet_encrypt(UDF_INIT *initid, UDF_ARGS *args, char *result, unsigned long *length, char *is_null, char *error);
void safenet_encrypt_deinit(UDF_INIT *initid);

my_bool safenet_decrypt_init(UDF_INIT *initid, UDF_ARGS *args, char *message);
char *safenet_decrypt(UDF_INIT *initid, UDF_ARGS *args, char *result, unsigned long *length, char *is_null, char *error);
void safenet_decrypt_deinit(UDF_INIT *initid);

my_bool safenet_config_init(UDF_INIT *initid, UDF_ARGS *args, char *message);
char *safenet_config(UDF_INIT *initid, UDF_ARGS *args, char *result, unsigned long *length, char *is_null, char *error);
void safenet_config_deinit(UDF_INIT *initid);		

```

safenet.c

```

/*
Homepage: http://netkiller.github.io/
Author: netkiller<netkiller@msn.com>
*/

#include <mysql.h>
#include <string.h>

#include <stdio.h>
#include <stdlib.h>
#include <curl/curl.h>
#include "safenet.h"

#define SAFENET_URL "http://localhost/safe/interface" 
#define SAFENET_KEY "Web01-key" 

char *safe_url;
char *safe_key;

void get_safenet_env(){
    if (getenv("SAFENET_URL")){
	safe_url = getenv("SAFENET_URL");
    }else{
	safe_url = SAFENET_URL;
    }
    if (getenv("SAFENET_KEY")){
	safe_key = getenv("SAFENET_KEY");
    }else{
	safe_key = SAFENET_KEY;
    }
}

/* CURL FUNCTION BEGIN*/
struct string {
  char *ptr;
  size_t len;
};

void init_string(struct string *s) {
  s->len = 0;
  s->ptr = malloc(s->len+1);
  if (s->ptr == NULL) {
    fprintf(stderr, "malloc() failed\n");
    exit(EXIT_FAILURE);
  }
  s->ptr[0] = '\0';
}

size_t writefunc(void *ptr, size_t size, size_t nmemb, struct string *s)
{
  size_t new_len = s->len + size*nmemb;
  s->ptr = realloc(s->ptr, new_len+1);
  if (s->ptr == NULL) {
    fprintf(stderr, "realloc() failed\n");
    exit(EXIT_FAILURE);
  }
  memcpy(s->ptr+s->len, ptr, size*nmemb);
  s->ptr[new_len] = '\0';
  s->len = new_len;

  return size*nmemb;
}

char * safenet(char *url, char *mode, char *key, char *in )
{ 
    CURL *curl;
    CURLcode res;
    char *fields;
    char *data;

//  curl_global_init(CURL_GLOBAL_ALL);

    /* get a curl handle */ 
    curl = curl_easy_init();
    if(curl) {
        struct string s;
        init_string(&s); 

        asprintf(&fields, "mode=%s&keyname=%s&input=%s", mode, key, in);    

        curl_easy_setopt(curl, CURLOPT_URL, url);
        curl_easy_setopt(curl, CURLOPT_USERAGENT, "safenet/1.0 by netkiller <netkiller@msn.com>");
        curl_easy_setopt(curl, CURLOPT_WRITEFUNCTION, writefunc);
        curl_easy_setopt(curl, CURLOPT_WRITEDATA, &s);
        curl_easy_setopt(curl, CURLOPT_POSTFIELDS, fields);

        /* Perform the request, res will get the return code */ 
        res = curl_easy_perform(curl);
        /* Check for errors */ 
        if(res != CURLE_OK)
          fprintf(stderr, "curl_easy_perform() failed: %s\n",
                  curl_easy_strerror(res));

        asprintf(&data, "%s", s.ptr);
        //printf("Encrypt: %s\n", data);

        free(s.ptr);
        /* always cleanup */ 
        curl_easy_cleanup(curl);
    }
    else{
	strcpy(data,"");
    }

    return data;
  //curl_global_cleanup();
}
/* CURL FUNCTION END*/

/* ------------------------ safenet encrypt ----------------------------- */

my_bool safenet_encrypt_init(UDF_INIT *initid, UDF_ARGS *args, char *message)
{

  if (args->arg_count != 1)
  {
    strncpy(message,
            "two arguments must be supplied: safenet_encrypt('<data>').",
            MYSQL_ERRMSG_SIZE);
    return 1;
  }
  get_safenet_env(); 
  args->arg_type[0]= STRING_RESULT;

  return 0;
}

char *safenet_encrypt(UDF_INIT *initid, UDF_ARGS *args,
                __attribute__ ((unused)) char *result,
               unsigned long *length,
                __attribute__ ((unused)) char *is_null,
                __attribute__ ((unused)) char *error)
{

    char *data;
    data = safenet(safe_url, "encrypt", safe_key, args->args[0]);
    *length = strlen(data);
    return ((char *)data);

}

void safenet_encrypt_deinit(UDF_INIT *initid)
{
  return;
}

/* ------------------------ safenet decrypt ----------------------------- */

my_bool safenet_decrypt_init(UDF_INIT *initid, UDF_ARGS *args, char *message)
{

  if (args->arg_count != 1)
  {
    strncpy(message,
            "two arguments must be supplied: safenet_decrypt('<data>').",
            MYSQL_ERRMSG_SIZE);
    return 1;
  }

  get_safenet_env();
  args->arg_type[0]= STRING_RESULT;

  return 0;
}

char *safenet_decrypt(UDF_INIT *initid, UDF_ARGS *args,
                __attribute__ ((unused)) char *result,
               unsigned long *length,
                __attribute__ ((unused)) char *is_null,
                __attribute__ ((unused)) char *error)
{

    char *data;
    if(strlen(args->args[0]) != 512){
        data = args->args[0];
    }else{
        data = safenet(safe_url, "decrypt", safe_key, args->args[0]);
    }
    *length = strlen(data);
    return ((char *)data);

}

void safenet_decrypt_deinit(UDF_INIT *initid)
{
  return;
}

/* ------------------------ safenet config ----------------------------- */

my_bool safenet_config_init(UDF_INIT *initid, UDF_ARGS *args, char *message)
{

    get_safenet_env();
    return 0;
}

char *safenet_config(UDF_INIT *initid, UDF_ARGS *args,
                __attribute__ ((unused)) char *result,
               unsigned long *length,
                __attribute__ ((unused)) char *is_null,
                __attribute__ ((unused)) char *error)
{

  char *config;
  asprintf(&config, "SAFENET_URL=%s, SAFENET_KEY=%s", safe_url, safe_key);
  *length = strlen(config);
  return ((char *)config);
}

void safenet_config_deinit(UDF_INIT *initid)
{
   return;
}

```

CMakeLists.txt

```

cmake_minimum_required(VERSION 2.8)
PROJECT(safenet)
ADD_LIBRARY(safenet SHARED safenet.c)
INCLUDE_DIRECTORIES(/usr/include/mysql)
TARGET_LINK_LIBRARIES(safenet curl)
INSTALL(PROGRAMS libsafenet.so DESTINATION /usr/lib64/mysql/plugin/)

```

Installation Plugin

```

yum install -y libcurl-devel

cd src
cmake .
make 
make install

cat > /etc/sysconfig/mysqld <<EOF
export SAFENET_URL=http://host.localdomain/safe/interface
export SAFENET_KEY=Web01-key
EOF

```

Create Function

```

create function safenet_encrypt returns string soname 'libsafenet.so';
create function safenet_decrypt returns string soname 'libsafenet.so';
create function safenet_config returns string soname 'libsafenet.so';

```

Example

```

mysql> select safenet_encrypt('Helloworld!!!');
+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| safenet_encrypt('Helloworld!!!')                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 994BAB7BC417F0559A09ECE94EDCB695AC1D5705F7ABA9F3562158F5AFAC4720FA9B3E53F30DF65C1726E0F02A93A9CAE7E486349F41AE4F504DC2B49F809C5AF77FEF4DE49D03D8DEC4000B15F2F2A2296500AA6159491E65DEFDFE75FB2E79D31D9BF0CC67932ADA212C34C0B04BF30F222102FAD857F440404C0FE92B8626EA3126B0B5A4FA0B1D09F1CC9EF45EBB6A72123AE82D39F659C717A5AA4F7FB5BDBBC7977C7021F61BBC26B9DB78C9A8657C6BC291CAE5C07F9DF485D71A1E9CC8888793B03BB5AF2DDB57AAEFB6D2EA569226651092414F96BA0880B35B0D8A01A1F7B82C308A2316D07C0FD4E0A298ECB33F4E4EB9F1A1E53760B0BFBE7449 |
+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
1 row in set (0.58 sec)

mysql> select safenet_decrypt(safenet_encrypt('Helloworld!!!'));
+---------------------------------------------------+
| safenet_decrypt(safenet_encrypt('Helloworld!!!')) |
+---------------------------------------------------+
| Helloworld!!!                                     |
+---------------------------------------------------+
1 row in set (0.31 sec)

mysql> select safenet_config();	

```

Drop Function

```

drop function safenet_encrypt;
drop function safenet_decrypt;
drop function safenet_config;	

```

## 数据区块链

背景：例如我们需要一个排行榜，存储活动的报名顺序或者考试成绩。我们防止有人作弊或者撰改，包括 DBA 在内。

任务：1.数据检查，2.发现撰改，2.风险提示

方案：使用链表指针方案,将数据看成一个链条，中间任何改动，就如同链条被剪断，改动之处之后的数据全部视为无效。

结果：达到数据后发现是否撰改，提示风险目的

```

CREATE TABLE `top100_list` (
	`id` INT,
	`name` VARBINARY(16) NOT NULL,
	......
	......
	`extend` VARCHAR(32) NULL
)
ENGINE=InnoDB;

```

演示数据

```

id | extend | ...
1 | 0 | ...
2 | 1 | ...
3 | 2 | ...
4 | 3 | ...		
5 | 4 | ...

```

extend 始终集成上一条记录，保证数据是连续的。但这样还不够，这样只能防止数据被删除，如果其他字段被修改呢

```

id | extend | ...
1 | NULL | ...
2 | crc32(...) | ...
3 | crc32(...) | ...
4 | crc32(...) | ...		
5 | crc32(...) | ...

```

我们使用 crc 算法运算上一条一整行的数据，你还可以使用 salt 技术干扰，这个 salt 只有软件部署者知道，DBA 和开发人员不得而知。

对于一般数据 crc32 可能做到性能和安全性平衡，如果安全要求更高可以使用 sha256 等等，甚至采用 RSA 非对称秘钥。

## 状态保护

表中有一个 Status 字段，是一个状态机，你可以理解为工作流，工作流是有任务流向的，不能随意修改其状态。

```

CREATE TABLE `card` (
	`id` INT(10) UNSIGNED NOT NULL AUTO_INCREMENT,
	`uuid` VARCHAR(36) NOT NULL COMMENT 'UUID' COLLATE 'utf8mb4_unicode_ci',
	`number` VARCHAR(36) NOT NULL COMMENT '充值卡号码' COLLATE 'utf8mb4_unicode_ci',
	`price` MEDIUMINT(8) UNSIGNED NOT NULL COMMENT '面值',
	`status` ENUM('New','Activated','Recharged','Discard') NOT NULL DEFAULT 'New' COMMENT '充值卡状态' COLLATE 'utf8mb4_unicode_ci',
	`ctime` TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
	`mtime` TIMESTAMP NULL DEFAULT NULL ON UPDATE CURRENT_TIMESTAMP COMMENT '修改时间',
	PRIMARY KEY (`id`),
	UNIQUE INDEX `number` (`number`),
	UNIQUE INDEX `uuid` (`uuid`)
)
COMMENT='充值卡表'
COLLATE='utf8mb4_unicode_ci'
ENGINE=InnoDB
AUTO_INCREMENT=1;

```

状态流向

```

	+----------+    +-----------+    +-----------+    
	| New      | -> | Activated | -> | Recharged |
	+----------+    +-----------+    +-----------+    
		 |                |
		 V                |
	+----------+          |
    | Discard  | <--------+
	+----------+

```

为此我们创建触发器保护状态正确走向。

```

CREATE DEFINER=`root`@`%` TRIGGER `card_before_update` BEFORE UPDATE ON `card` FOR EACH ROW BEGIN
	set new.uuid 		= old.uuid;
	set new.number		= old.number;
	set new.price		= old.price;
	set new.ctime		= old.ctime;

	IF old.status = "New" THEN
		IF new.status NOT IN ("Activated","Discard") THEN
			SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Status denied', MYSQL_ERRNO = 1001;
		END IF;
	END IF;
	IF old.status = "Activated" THEN
		IF new.status NOT IN ("Recharged") THEN
			SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Status denied', MYSQL_ERRNO = 1001;
		END IF;
	END IF;
	IF old.status = "Recharged" THEN
		set new.status	= old.status;
	END IF;
END

```

保护记录不被删除

```

CREATE DEFINER=`root`@`%` TRIGGER `card_before_delete` BEFORE DELETE ON `card` FOR EACH ROW BEGIN
	SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Permission denied', MYSQL_ERRNO = 1001;
END

```

这个方案很容易移植到其他场景中，例如购物，发货，收货等等

## 数据归档

MySQL 提供 ARCHIVE 引擎，ARCHIVE 归档的数据不能够修改，这个引擎只提供插入操作

```

CREATE TABLE `logging` (
	`id` INT(10) UNSIGNED NOT NULL AUTO_INCREMENT,
	`tag` ENUM('unknow','www','user','admin') NOT NULL DEFAULT 'unknow' COMMENT '日志标签',
	`time` TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '产生时间',
	`facility` ENUM('card','payment','sms','blockchain') NOT NULL COMMENT '类别',
	`priority` ENUM('info','warning','error','critical','exception','debug') NOT NULL COMMENT '级别',
	`message` VARCHAR(1024) NOT NULL COMMENT '内容',
	PRIMARY KEY (`id`)
)
COMMENT='日志表'
COLLATE='utf8_general_ci'
ENGINE=ARCHIVE
AUTO_INCREMENT=1;

```

## 第 6 章 Sharding

Sharding 是近几年提出的概念，可以做分表，分库切割，通过 hash 值定位。但都存在一个问题，数据连续性，索引无法跨表。

Oracle 在 8.x 中就支持分区功能，MySQL 在 5.1.x 中也是闲类似功能，PostgreSQL 因存储结构设计的较好，基本不需要做分区。

## horizontal

```
ALTER TABLE `goods`  DROP INDEX `goods_sn_2`;
ALTER TABLE goods PARTITION BY RANGE (goods_id) (
    PARTITION p0 VALUES LESS THAN (10000),
    PARTITION p1 VALUES LESS THAN (20000),
    PARTITION p2 VALUES LESS THAN (30000),
    PARTITION p3 VALUES LESS THAN (40000),
    PARTITION p4 VALUES LESS THAN MAXVALUE
);

ALTER TABLE goods PARTITION BY HASH(goods_id) PARTITIONS 10;

ALTER TABLE goods  PARTITION BY KEY (is_on_sale) PARTITIONS 2;

ALTER TABLE goods PARTITION BY HASH(YEAR(FROM_UNIXTIME(add_time))) PARTITIONS 4;

```

## vertical

## 新闻数据库分表案例

这里我通过一个新闻网站为例，解决分表的问题

避免开发中经常拼接表，我采用一个一劳永逸的方法，建立一个 news 表使用黑洞引擎，然后通过出发器将数据分流到匹配的表中。同时采用 uuid 替代数字序列，可以保证未来数年不会出现 ID 用尽。

```

CREATE TABLE IF NOT EXISTS `news` (
  `uuid` varchar(36) NOT NULL COMMENT '唯一 ID',
  `title` varchar(50) NOT NULL COMMENT '新闻标题',
  `body` text NOT NULL COMMENT '新闻正文',
  `ctime` timestamp NOT NULL DEFAULT '0000-00-00 00:00:00' COMMENT '创建时间',
  `mtime` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '修改时间',
  `atime` timestamp NOT NULL DEFAULT '0000-00-00 00:00:00' COMMENT '访问时间',
  PRIMARY KEY (`uuid`)
) ENGINE=BLACKHOLE DEFAULT CHARSET=utf8;

```

该表仅仅用于举例，结构比较简单。接下来创建年份分表，你也可以每个月一个表，根据你的许下灵活调整。表结构与上面的 news 表相同，注意 ENGINE=InnoDB。

```

CREATE TABLE IF NOT EXISTS `news_2012` (
  `uuid` varchar(36) NOT NULL COMMENT '唯一 ID',
  `title` varchar(50) NOT NULL COMMENT '新闻标题',
  `body` text NOT NULL COMMENT '新闻正文',
  `ctime` timestamp NOT NULL DEFAULT '0000-00-00 00:00:00' COMMENT '创建时间',
  `mtime` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '修改时间',
  `atime` timestamp NOT NULL DEFAULT '0000-00-00 00:00:00' COMMENT '访问时间',
  PRIMARY KEY (`uuid`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='news 表';

CREATE TABLE IF NOT EXISTS `news_2013` (
  `uuid` varchar(36) NOT NULL COMMENT '唯一 ID',
  `title` varchar(50) NOT NULL COMMENT '新闻标题',
  `body` text NOT NULL COMMENT '新闻正文',
  `ctime` timestamp NOT NULL DEFAULT '0000-00-00 00:00:00' COMMENT '创建时间',
  `mtime` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '修改时间',
  `atime` timestamp NOT NULL DEFAULT '0000-00-00 00:00:00' COMMENT '访问时间',
  PRIMARY KEY (`uuid`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='news 表';

```

uuid 索引表，主要的功能是通过 uuid 查询出该记录在那张表中。更好的方案是将数据放入 solr 中处理，包括标题与内容搜索等等。

```

CREATE TABLE `news_index` (
	`uuid` VARCHAR(36) NOT NULL,
	`tbl_name` VARCHAR(10) NOT NULL,
	PRIMARY KEY (`uuid`)
)
COMMENT='news uuid 索引表'
COLLATE='utf8_general_ci'
ENGINE=InnoDB;

```

news_insert 过程，用于向目标表中插入数据，可以单独 call 但不建议。因为 insert 远比 call 更通用，要考虑移植性与通用性

```

DELIMITER //
CREATE DEFINER=`neo`@`%` PROCEDURE `news_insert`(IN `uuid` vARCHAR(36), IN `title` VARCHAR(50), IN `body` TEXT, IN `ctime` TIMESTAMP)
BEGIN
	if year(ctime) = '2012' then
		insert into news_2012(uuid,title,body,ctime) values(uuid,title, body, ctime);
	end if;
	if year(ctime) = '2013' then
		insert into news_2013(uuid,title,body,ctime) values(uuid,title, body, ctime);
	end if;
	insert into news_index values(uuid, year(ctime));
END//
DELIMITER ;

```

插入触发器，负责获取 uuid 然后调用存储过程

```

SET @OLDTMP_SQL_MODE=@@SQL_MODE, SQL_MODE='';
DELIMITER //
CREATE TRIGGER `news_before_insert` BEFORE INSERT ON `news` FOR EACH ROW BEGIN
	IF new.uuid is null or new.uuid = '' or length(new.uuid) != 36 THEN
		set new.uuid=uuid();
	END IF;
	call news_insert(new.uuid,new.title,new.body,new.ctime);
END//
DELIMITER ;
SET SQL_MODE=@OLDTMP_SQL_MODE;

```

这个触发器用户保护表中的 uuid 值不被修改。

```

SET @OLDTMP_SQL_MODE=@@SQL_MODE, SQL_MODE='';
DELIMITER //
CREATE TRIGGER `news_before_update` BEFORE UPDATE ON `news_2013` FOR EACH ROW BEGIN
	set new.uuid = old.uuid;
END//
DELIMITER ;
SET SQL_MODE=@OLDTMP_SQL_MODE;

```

## 第 7 章 数据库并行访问控制

这里主要讲述有关开发中遇到的数据库并行问题

## 防止并行显示

背景

我们有一个 order 订单表，工作流如下

```

创建订单 -> 订单分配 -> 订单审核 -> 批准 -> 发货 ... 等等			

```

有多个岗位，每个岗位上有多个工作人员。需要实现相同岗位上的工作人员看到的订单不能重复，防止多人同时操作一个订单。

```
id | user | sn    | status
-----------------------------------
1  | neo  | x001  | new
2  | jam  | x002  | new
3  | sam  | x003  | new
4  | tom  | x004  | new
5  | ann  | x005  | new
6  | leo  | x006  | new
7  | ant  | x007  | new
8  | cat  | x008  | new

```

正常情况只要是多人一起打开订单页面就会显示上面的订单，并且每个人显示的内容都相同。

```
CREATE TABLE `orders` (
	`id` INT(10) UNSIGNED NOT NULL AUTO_INCREMENT,
	`name` VARCHAR(50) NOT NULL,
	`sn` INT(10) UNSIGNED ZEROFILL NOT NULL,
	`status` ENUM('New','Pending','Processing','Success','Failure') NOT NULL DEFAULT 'New',
	PRIMARY KEY (`id`),
	UNIQUE INDEX `sn` (`sn`)
)
COMMENT='订货单'
COLLATE='utf8_general_ci'
ENGINE=InnoDB	

```

```
INSERT INTO `orders` (`id`, `name`, `sn`, `status`) VALUES
	(1, 'neo', 0000000001, 'New'),
	(2, 'jam', 0000000002, 'New'),
	(3, 'sam', 0000000003, 'New'),
	(4, 'tom', 0000000004, 'New'),
	(5, 'ann', 0000000005, 'New'),
	(6, 'leo', 0000000006, 'New'),
	(7, 'ant', 0000000007, 'New'),
	(8, 'cat', 0000000008, 'New');

```

表 7.1. 工作流模拟

| 操作 | 订单审核员 A | 订单审核员 B |
| --- | --- | --- |
| 显示未处理订单,这里模拟两个人同时点开页面的情景 |  
```
begin;
select id from orders where status='New' limit 5 for update;
update orders set status='Pending' where status='New' and id in (1,2,3,4,5);
select * from orders where status='Pending' and id in (1,2,3,4,5) order by id asc limit 5;
commit;

```

首先查询出数据库中的前五条记录，然后更新为 Pending 状态，防止他人抢占订单。 |  
```
begin;
select id from orders where status='New' limit 5 for update;
update orders set status='Pending' where status='New' and id in (6,7,8);
select * from orders where status='Pending' and id in (6,7,8) order by id asc limit 5;
commit;

```

select 的时候会被行级所挂起，直到被 commit 后才能查询出新数据，这是显示的数据是剩下的后 5 条 |
| 处理订单，模拟两个人点击审批通过按钮是的情景 |  
```
begin;							
select * from orders where status='Pending' and id='1' for update;
update orders set status='Processing' where status='Pending' and id=1;
commit;

```

更新状态 Pending 到 Processing |  
```
begin;							
select * from orders where status='Pending' and id='6' for update;
update orders set status='Processing' where status='Pending' and id=6;
commit;

```

更新状态 Pending 到 Processing |
| 处理成功与失败的情况 |  
```
begin;							
select * from orders where status='Processing' and id='1' for update;
update orders set status='Success' where status='Processing' and id=1;
commit;

```

 |  
```
begin;							
select * from orders where status='Processing' and id='6' for update;
update orders set status='Failure' where status='Processing' and id=6;
commit;

```

 |
| 处理 Pending 状态的订单，可能产生冲突，不用担心有行锁，防止重复处理。 |  
```
begin;							
select * from orders where status='Processing' and id='5' for update;
update orders set status='Failure' where status='Processing' and id=5;
commit;

```

 |  
```
begin;							
select * from orders where status='Processing' and id='5' for update;
update orders set status='Failure' where status='Processing' and id=5;
commit;

```

 |

有一种情况，用户查看了列表并未及时处理订单，就会有很多 Pending 状态的订单，这是需要有人处理这些订单，但查询 Pending 时，可能同一时刻有人在审批订单，我们通过排他锁避免重复处理。

上面以 MySQL 为例，每次都需要使用 for update 查出要处理的订单，如果是 PostgreSQL 可以使用 update + returning 来返回修改的数据，更为方便。

## 第 8 章 数据与应用程序间通信

本章讲解数据与应用程序间通信，这里会涉及到

## 管道通信

你是否想过当数据库中的数据发生变化的时候出发某种操作？但因数据无法与其他进程通信（传递信号）让你放弃，而改用每隔一段时间查询一次数据变化的方法？下面的插件可以解决你的问题。

### 背景

你是否有这样的需求：

你需要监控访问网站的 IP，当同一个 IP 地址访问次数过多需要做出处理，例如拉黑，直接丢进 iptables 防火墙规则连中。你的做法只能每个一段时间查询一次数据库，并且判断是否满足拉黑需求？

你是否需要监控某些数据发生变化，并通知其他程序作出处理。例如新闻内容修改后，需要立即做新页面静态化处理，生成新的静态页面

你使用数据库做队列，例如发送邮件，短信等等。你要通知发送程序对那些手机或者短线发送数据

### 解决思路

需要让数据库与其他进程通信，传递信号

例如，发送短信这个需求，你只要告诉发短信的机器人发送的手机号码即可，机器人永远守候那哪里，只要命令一下立即工作。

监控数据库变化的需求原理类似，我们需要有一个守护进程等待命令，一旦接到下达命令便立即生成需要的静态页面

这里所提的方案是采用 fifo(First In First Out)方案，通过管道相互传递信号，使两个进程协同工作，这样的效率远比定时任务高许多。fifo 是用于操作系统内部进程间通信，如果跨越操作系统需要使用 Socket，还有一个新名词 MQ(Message queue).

这里只做 fifo 演示, 将本程序改为 Socket 方案，或者直接集成成熟的 MQ 也是分分钟可以实现。

### Mysql plugin

我开发了几个 UDF, 共 4 个 function

UDF

fifo_create(pipename)

创建管道.成功返回 true,失败返回 flase.

fifo_remove(pipename)

删除管道.成功返回 true,失败返回 flase.

fifo_read(pipename)

读操作.

fifo_write(pipename,message)

写操作 pipename 管道名,message 消息正文.

有了上面的 function 后你就可以在 begin,commit,rollback 直接穿插使用，实现在事物处理期间做你爱做的事。也可以用在触发器与 EVENT 定时任务中。

### plugin 的开发与使用

编译 UDF 你需要安装下面的软件包

```
sudo apt-get install pkg-config
sudo apt-get install libmysqlclient-dev

sudo apt-get install gcc gcc-c++ make automake autoconf

```

[`github.com/netkiller/mysql-fifo-plugin`](https://github.com/netkiller/mysql-fifo-plugin)

编译 udf，最后将 so 文件复制到 /usr/lib/mysql/plugin/

```
git clone https://github.com/netkiller/mysql-image-plugin.git
cd mysql-image-plugin

gcc -O3  -g  -I/usr/include/mysql -I/usr/include  -fPIC -lm -lz -shared -o fifo.so fifo.c
sudo mv fifo.so /usr/lib/mysql/plugin/

```

装载

```
create function fifo_create returns string soname 'fifo.so';
create function fifo_remove returns string soname 'fifo.so';
create function fifo_read returns string soname 'fifo.so';
create function fifo_write returns string soname 'fifo.so';

```

卸载

```
drop function fifo_create;
drop function fifo_remove;
drop function fifo_read;
drop function fifo_write;

```

### 插件如何使用

插件有很多种用法，这里仅仅一个例

```
CREATE TABLE `demo` (
	`id` INT(11) NULL DEFAULT NULL,
	`name` CHAR(10) NULL DEFAULT NULL,
	`mobile` VARCHAR(50) NULL DEFAULT NULL
)
COLLATE='utf8_general_ci'
ENGINE=InnoDB;

INSERT INTO `demo` (`id`, `name`, `mobile`) VALUES
	(1, 'neo', '13113668891'),
	(2, 'jam', '13113668892'),
	(3, 'leo', '13113668893');

```

我们假设有一个 demo 这样的表,我使用 shell 写了一个守护进程用于处理数据库送过来的数据

```

#!/bin/bash
########################################
# Homepage: http://netkiller.github.io
# Author: neo <netkiller@msn.com>
########################################
NAME=demo
PIPE=/tmp/myfifo
########################################
LOGFILE=/tmp/$NAME.log
PIDFILE=/tmp/${NAME}.pid
########################################

function start(){
	if [ -f "$PIDFILE" ]; then
		exit 2
	fi

        if [ ! -f "$LOGFILE" ]; then
                > ${LOGFILE}
        fi

	for (( ; ; ))
	do
            while read line
            do
				NOW=$(date '+%Y-%m-%d %H:%M:%S')

                echo "[${NOW}] [OK] ${line}" >> ${LOGFILE}

            done < $PIPE
	done &
	echo $! > $PIDFILE
}
function stop(){
  	[ -f $PIDFILE ] && kill `cat $PIDFILE` && rm -rf $PIDFILE
}

case "$1" in
  start)
  	start
	;;
  stop)
  	stop
	;;
  status)
  	ps ax | grep ${0} | grep -v grep | grep -v status
	;;
  restart)
  	stop
	start
	;;
  *)
	echo $"Usage: $0 {start|stop|status|restart}"
	exit 2
esac

exit $?

```

启动守护进程

```
$ ./sms.sh start
$ ./sms.sh status
  596 pts/5    S      0:00 /bin/bash ./sms.sh start

```

监控日志，因为守护进程没有输出，完成人户后写入日志。

```
$ tail -f /tmp/demo.log

```

开始推送任务

```

mysql> select fifo_write('/tmp/myfifo',concat(mobile,'\r\n')) from demo;
+-------------------------------------------------+
| fifo_write('/tmp/myfifo',concat(mobile,'\r\n')) |
+-------------------------------------------------+
| true                                            |
| true                                            |
| true                                            |
+-------------------------------------------------+
3 rows in set (0.00 sec)

```

现在看看日志的变化

```
$ tail -f /tmp/demo.log
[2013-12-16 14:55:48] [OK] 13113668891
[2013-12-16 14:55:48] [OK] 13113668892
[2013-12-16 14:55:48] [OK] 13113668893

```

我们再将上面的例子使用触发器进一步优化

```

CREATE TABLE `demo_sent` (
	`id` INT(10) UNSIGNED NOT NULL AUTO_INCREMENT,
	`mobile` VARCHAR(50) NOT NULL,
	`status` ENUM('true','false') NOT NULL DEFAULT 'false',
	`ctime` TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
	PRIMARY KEY (`id`)
)
COLLATE='utf8_general_ci'
ENGINE=InnoDB

CREATE DEFINER=`dba`@`%` TRIGGER `demo_after_insert` AFTER INSERT ON `demo` FOR EACH ROW BEGIN
	insert into demo_sent(mobile,status) select new.mobile,fifo_write('/tmp/myfifo',concat(new.mobile,'')) as status;
END

```

测试

```

mysql> insert into demo(name,mobile) values('jerry','13322993040');
Query OK, 1 row affected (0.05 sec)		

```

日志变化

```
$ tail -f /tmp/demo.log 
[2013-12-16 14:55:48] [OK] 13113668891
[2013-12-16 14:55:48] [OK] 13113668892
[2013-12-16 14:55:48] [OK] 13113668893
[2013-12-16 14:55:48] [OK] 13322993040

```

### 部署相关问题

我们可以采用主从数据库，将任务放在专用的从库上执行

我们可以创建很多个管道，用于做不同的工作，例如插入一个任务，更新一个任务，发短信一个任务，处理模板与静态化一个任务等等。

## 消息队列

这里选择使用 ZeroMQ 的原因主要考虑的是性能问题，其他 MQ 方案可能会阻塞数据库。

### 背景

之前我发表过一篇文章 http://netkiller.github.io/journal/mysql.plugin.fifo.html

该文章中提出了通过 fifo 管道，实现数据库与其他进程的通信。属于 IPC 机制(同一个 OS/服务器内)，后我有采用 ZeroMQ 重新实现了一个 RPC 机制的方案，同时兼容 IPC（跨越 OS/服务器）

各种缩写的全称 IPC(IPC :Inter-Process Communication 进程间通信)，ITC(ITC : Inter Thread Communication 线程间通信)与 RPC(RPC: Remote Procedure Calls 远程过程调用)。

支持协议

```
inproc://my_publisher
tcp://server001:5555
ipc:///tmp/feeds/0

```

### 应用场景

如果你想处理数据，由于各种原因你不能在程序中实现，你可以使用这个插件。当数据库中的数据发生变化的时候出发某种操作,你可以使用这个插件。

有时候你的项目可能是外包的，项目结束后外包方不会在管你，你有无法改动现有代码，或者根本不敢改。你可以使用这个插件

采用 MQ 技术对数据库无任何压力，与采用程序处理并无不同，省却了写代码

处理方法，可以采用同步或者异步方式

例 8.1. 发送短信

发送短信、邮件，只需要查询出相应手机号码，发送到 MQ 的服务端，服务端接收到手机号码后，放入队列中，多线程程序从队列中领取任务，发送短信。

```
select zmq_client('tcp://localhost:5555',mobile) from demo where subscribed='Y' ...;

```

传递多个参数，可以使用符号分隔

```
select zmq_client('tcp://localhost:5555',concat(name,',',mobile,', news')) from demo;
select zmq_client('tcp://localhost:5555',concat(name,'|',mobile,'|news')) from demo;

```

json 格式

```
select zmq_client('tcp://localhost:5555',concat('{name:',name,', tel:',mobile,', template:news}')) from demo;

```

建议采用异步方式，MQ 端接收到任务立即反馈 “成功”信息，因为我们不太关心是否能发送成功，本身就是盲目性的发送，手机号码是否可用我们无从得知，短信或者邮件的发送到达率不是 100%，所以当进入队列后，让程序自行处理，将成功或者失败信息记录到日志中即可。

例 8.2. 处理图片

首先查询出需要处理图片，然后将路径与分辨率传递给 MQ 另一端的处理程序

```
select zmq_client('tcp://localhost:5555',concat(image,',800x600}')) from demo;

```

建议采用异步方式，MQ 端接收到任务立即反馈 “成功”信息

例 8.3. 身份证号码校验

```
select zmq_client('tcp://localhost:5555',id_number) from demo;

```

可以采用同步方案，因为 MQ 款处理几乎不会延迟，直接将处理结构反馈

例 8.4. 静态化案例

情景模拟，你的项目是你个电商项目，采用外包模式开发，项目已经开发完成。外包放不再负责维护，你现在要做静态化。增加该功能，你要检查多处与商品表相关的造作。

于其改代码，不如程序从外部处理，这样更保险。我们只要写一个程序将动态 URL 下载保存成静态即可，当数据发生变化的时候重新下载覆盖即可

```
CREATE DEFINER=`dba`@`%` TRIGGER `demo_after_insert` AFTER INSERT ON `demo` FOR EACH ROW BEGIN
	select zmq_client('tcp://localhost:5555', NEW.id);
END
CREATE DEFINER=`dba`@`%` TRIGGER `demo_after_update` AFTER UPDATE ON `demo` FOR EACH ROW BEGIN
	select zmq_client('tcp://localhost:5555', NEW.id);
END
CREATE DEFINER=`dba`@`%` TRIGGER `demo_after_delete` AFTER DELETE ON `demo` FOR EACH ROW BEGIN
	select zmq_client('tcp://localhost:5555', NEW.id);
END

```

MQ 另一端的服务会下载 http://www.example.com/goods.php?cid=111&id=100, 然后生成 html 页面，http://www.example.com/111/100.html

插入会新建页面，更新会覆盖页面，删除会删除页面

这样无论商品的价格，属性改变，静态化程序都会做出相应的处理。

例 8.5. 数据同步案例

我们有多个数据库，A 库里面的数据发生变化后，要同步书库到 B 库，或者处理结果，或者数据转换后写入其他数据库中

方法也是采用触发器或者 EVENT 处理

### Mysql plugin

我开发了几个 UDF, 共 4 个 function

UDF

zmq_client(sockt,message)

sockt .成功返回 true,失败返回 flase.

有了上面的 function 后你就可以在 begin,commit,rollback 直接穿插使用，实现在事物处理期间做你爱做的事。也可以用在触发器与 EVENT 定时任务中。

### plugin 的开发与使用

编译 UDF 你需要安装下面的软件包

```
sudo apt-get install pkg-config
sudo apt-get install libmysqlclient-dev

sudo apt-get install gcc gcc-c++ make cmake

```

[`github.com/netkiller/mysql-zmq-plugin`](https://github.com/netkiller/mysql-zmq-plugin)

编译 udf，最后将 so 文件复制到 /usr/lib/mysql/plugin/

```

git clone https://github.com/netkiller/mysql-zmq-plugin.git
cd mysql-zmq-plugin

cmake .
make && make install

```

装载

```
create function zmq_client returns string soname 'libzeromq.so';
create function zmq_publish returns string soname 'libzeromq.so';

```

卸载

```
drop function zmq_client;
drop function zmq_publish;

```

确认安装成功

```

mysql> SELECT * FROM `mysql`.`func` where name like 'zmq%';
+-------------+-----+--------------+----------+
| name        | ret | dl           | type     |
+-------------+-----+--------------+----------+
| zmq_client  |   0 | libzeromq.so | function |
| zmq_publish |   0 | libzeromq.so | function |
+-------------+-----+--------------+----------+
2 rows in set (0.00 sec)

```

### 插件如何使用

插件有很多种用法，这里仅仅一个例

编译 zeromq server 测试程序

```
cd test
cmake .
make

```

启动服务进程

```
./server

```

发送 Hello world!

```

mysql> select zmq_client('tcp://localhost:5555','Hello world!');
+---------------------------------------------------+
| zmq_client('tcp://localhost:5555','Hello world!') |
+---------------------------------------------------+
| Hello world! OK                                   |
+---------------------------------------------------+
1 row in set (0.01 sec)

```

查看服务器端是否接收到信息。

```
$ ./server
Received: Hello world!

```

我们再将上面的例子使用触发器进一步优化

```

mysql> select zmq_client('tcp://localhost:5555',mobile) from demo;
+-------------------------------------------+
| zmq_client('tcp://localhost:5555',mobile) |
+-------------------------------------------+
| 13113668891 OK                            |
| 13113668892 OK                            |
| 13113668893 OK                            |
| 13322993040 OK                            |
| 13588997745 OK                            |
+-------------------------------------------+
5 rows in set (0.03 sec)

```

服务器端已经接收到数据库发过来的信息

```
$ ./server
Received: Hello world!
Received: 13113668891
Received: 13113668892
Received: 13113668893
Received: 13322993040
Received: 13588997745

```

我们可以拼装 json 或者序列化数据，发送给远端

```

mysql> select zmq_client('tcp://localhost:5555',concat('{name:',name,', tel:',mobile,'}')) from demo;
+------------------------------------------------------------------------------+
| zmq_client('tcp://localhost:5555',concat('{name:',name,', tel:',mobile,'}')) |
+------------------------------------------------------------------------------+
| {name:neo, tel:13113668891} OK                                               |
| {name:jam, tel:13113668892} OK                                               |
| {name:leo, tel:13113668893} OK                                               |
| {name:jerry, tel:13322993040} OK                                             |
| {name:tom, tel:13588997745} OK                                               |
+------------------------------------------------------------------------------+
5 rows in set (0.03 sec)

```

返回数据取决于你服务端怎么编写处理程序，你可以返回 true/false 等等。

触发器以及事务处理，这里就不演示了

## 数据库与外界文件

你是是不是在开发中常常遇到，删除了数据库记录后，发现该记录对应的图片没有删除，或者删除了图片，数据库中仍有数据存在，你的网站脏数据（图片）成几何数增长，阅读下文这里为你提供了一个完美决方案。

### 背景

我以电商网站为例，一般的网站产品数据存放在数据库中，商品图片是上传到文件服务器，然后通过 http 服务器浏览商品图片。这是最基本的也是最常见做法。

稍复杂的方案是，如果图片数量庞大，会使用分布式文件系统方案。但是这些方案都不能保证数据的完整性，极易产生脏数据（垃圾数据）。脏数据是指当你删除了数据库表中的记录后，图片仍然存在，或者手工删除了图片，而数据库中的记录仍然存在。

将图片放入数据库中存放在 BLOB 的方法可以解决脏数据问题，典型的案例是公安的身份证系统。但这种方案的前提是，图片不能太大，数量不多，访问量不大。 这显然不适合电商网站。

2009 年我在走秀网工作，商品图片与缩图文件 900GB 到 2012 离职已经有 10TB，每天有成百上千的商品上架下架，很多商品下架后永远不会再上架，这些批量下架的商品数据不会删除，仅仅标记为删除，总是期望以后能继续使用，实际上再也不会有人过问，另一方面随着品类经理频繁更换，员工离职，这些商品会石沉大海，再也无人问均。这些商品所对应的图片也就脏数据主要来源。新的品类经理上任后，会重新拍照，上传新图片。

总之，删除数据库中的数据不能将图片删除就会产生脏数据。很多采用删除数据的时候去检查图片如果存在先删除图片，再删除数据的方法。这种方案也非完美解决方案，存在这图片先被删除，程序出错 SQL 没有运行，或者反之。

### 解决思路

如果删除图片能够成为事物处理中的一个环节，所有问题都能迎刃而解，可彻底解决脏数据的烦恼。

### 解决方案

mysql plugin 开发 udf。我写几个 function

UDF

image_check(filename)

检查图片是否存在.

image_remove(filename)

删除图片.

image_rename(oldfile,newfile)

更改图片文件名.

image_md5sum(filename)

md5sum 主要用户图片是否被更改过.

image_move(filename,filename)

移动图片的位置

有了上面的 function 后你就可以在 begin,commit,rollback 直接穿插使用，实现在事物处理期间做你爱做的事。

### plugin 的开发与使用

编译 UDF 你需要安装下面的软件包

```
sudo apt-get install pkg-config
sudo apt-get install libmysqlclient-dev

sudo apt-get install gcc gcc-c++ make automake autoconf

```

[`github.com/netkiller/mysql-image-plugin`](https://github.com/netkiller/mysql-image-plugin)

编译 udf，最后将 so 文件复制到 /usr/lib/mysql/plugin/

```
git clone https://github.com/netkiller/mysql-image-plugin.git
cd mysql-image-plugin/src

gcc -I/usr/include/mysql -I./ -fPIC -shared -o image.so image.c
sudo mv image.so /usr/lib/mysql/plugin/

```

装载

```
create function image_check returns boolean soname 'images.so';
create function image_remove returns boolean soname 'images.so';
create function image_rename returns boolean soname 'images.so';
create function image_md5sum returns string soname 'images.so';
create function image_move returns string soname 'images.so';

```

卸载

```
drop function image_check;
drop function image_remove;
drop function image_rename;
drop function image_md5sum;
drop function image_move;

```

### 在事务中使用该插件

插入图片流程，上传图片后，通过插件检查图片是否正确上传，然后插入记录

```
begin;
IF image_check('/path/to/images.jpg') THEN
	insert into images(product_id,thumbnail,original) values(1000,'thumbnail/path/to/images.jpg','original/path/to/images.jpg');
	commit;
ELSE
	image_remove('/path/to/images.jpg');
END IF
rollback;

```

删除商品采用 image_move 方案，当出现异常 rollback 后还可以还原被删除的图片

```
begin;
IF image_check('/path/to/images.jpg') THEN
	select thumbnail,original into @thumbnail,@original from images where id='1000' for delete;
	delete from images where id='1000';
	select image_move(@thumbnail,'recycle/path/to/');
	select image_move(@original,'recycle/path/to/');
	commit;
END IF

rollback;
select image_move('recycle/path/to/images.jpg','path/to/images.jpg');

```

我们可以使用 EVENT 定时删除回收站内的图片

```
image_remove('recycle/path/to/images.jpg');

```

### 通过触发器调用图片处理函数

通过触发器更能保证数据完整性

```
1\. insert 触发器的任务： 插入记录的时候通过 image_check 检查图片是否正常上传，如果非没有上传，数据插入失败。
2\. delete 触发器的任务： 检查删除记录的时候，首先去删除图片，删除成功再删除该记录。

```

触发器进一步优化

```
1\. insert 触发器的任务： 插入记录的时候通过 image_check 检查图片是否正常上传，如果非没有上传，数据插入失败。如果上传成功再做 image_md5sum 进行校验 100% 正确后插入记录
2\. delete 触发器的任务： 检查删除记录的时候，首先去改图片文件名，然后删除该记录，最后删除图片，删除成功。如果中间环境失败 记录会 rollback，图片会在次修改文件名改回来。100% 保险

```

## Socket 方式

TCP 方式还不如使用现在有的消息队列，所以数据库通过 Socket 与应用程序通信，我推荐 UDP 方式。

UDP 有个好处，丢出去就不管了，性能非常好。并且可以实现组播，广播。下面是 UDP 的例子

### UDP

https://github.com/netkiller/mysql-udp-plugin

下载 mysql-udp_sendto-plugin 然后编译安装代码

```

# cmake .
# make && make install

```

安装

```

create function udp_sendto returns string soname 'libudp_sendto.so';

```

卸载

```

drop function udp_sendto;

```

使用演示，首先使用 nc 命令监听一个 UDP 端口，用来接收数据库发送过来的数据。数据结构请自行定义。这里仅仅是演示，可以采用 json, 逗号分隔等等方式。

```

# nc -luv 4000

```

在数据库中使用下面 SQL 发送数据给应用程序

```

select udp_sendto('192.168.2.1','4000','hello');

```

## 第 9 章 NoSQL OOD(Object-Oriented Design)

## MongoDB

### 配置表 config

```
{
    "_id" : ObjectId("5799a8535a855eca473977e1"),
    "key" : "payment",
    "value" : {
        "alpay" : true,
        "tenpay" : false,
        "unionpay" : false,
    }
},

{
    "_id" : ObjectId("5799a8535a855eca47397723"),
    "key" : "signup",
    "value" : {
        "online" : true,
        "manual" : false
    }
}

```

### 日志表

tag ENUM('unknow','www','user','admin') '日志标签',
time '插入时间',
facility ENUM('config','unionpay','sms','email') '类别',
priority ENUM('info','warning','error','critical','exception','debug') '级别',
message '内容'

```
/* 1 */
{
    "_id" : ObjectId("5799b0175a855eca473977e2"),
    "tag" : "www",
    "time" : "2016-07-30 12:12:00",
    "facility" : "config",
    "priority" : "info",
    "message" : "xxxxxxxxxxxxxxxxxx"
}

/* 2 */
{
    "_id" : ObjectId("5799b10f5a855eca473977e3"),
    "tag" : "www",
    "time" : "2016-07-30 12:12:00",
    "facility" : "config",
    "priority" : "info",
    "message" : {
        "name" : "neo",
        "age" : 30,
        "sex" : true
    }
}

```

## Cassandra

  <Keyspaces>
    <Keyspace Name="Example">

      <KeysCachedFraction>0.01</KeysCachedFraction>

      <ColumnFamily CompareWith="BytesType" Name="User"/>
      <ColumnFamily CompareWith="UTF8Type" Name="UserProfile"/>
      <ColumnFamily CompareWith="UTF8Type" Name="Category"/>
      <ColumnFamily CompareWith="UTF8Type" Name="Article"/>
      <ColumnFamily CompareWith="UTF8Type" Name="ArticleComment" />
      <ColumnFamily CompareWith="UTF8Type" Name="Product"/>
      <ColumnFamily CompareWith="UTF8Type" Name="ProductComment" />
      <ColumnFamily CompareWith="UTF8Type" Name="ProductAttribute" CompareSubcolumnsWith="UTF8Type" ColumnType="Super" />
      <ColumnFamily ColumnType="Super"
                    CompareWith="UTF8Type"
                    CompareSubcolumnsWith="UTF8Type"
                    Name="Address"
                    Comment="A column family with supercolumns, whose column and subcolumn names are UTF8 strings"/>
    </Keyspace>
  </Keyspaces>

### User And Profile

```
set Example.User['neo']['uuid']='b5ac78c3-fd5c-40ca-acc2-04d483052fc4'
set Example.User['neo']['name']='neo'
set Example.User['neo']['passwd']='mNBhMPAH'
set Example.User['neo']['email']='openunix@163.com'
set Example.User['neo']['status']='Y'

get Example.User['neo']

set Example.User['jam']['uuid']='8e07adbd-2dea-40d0-a822-5909f14f9ba2'
set Example.User['jam']['name']='jam'
set Example.User['jam']['passwd']='mNBhMPAH'
set Example.User['jam']['email']='t1@163.com'
set Example.User['jam']['status']='Y'

get Example.User['jam']

```

```
set Example.UserProfile['b5ac78c3-fd5c-40ca-acc2-04d483052fc4']['name']='neo chen'
set Example.UserProfile['b5ac78c3-fd5c-40ca-acc2-04d483052fc4']['age']='30'
set Example.UserProfile['b5ac78c3-fd5c-40ca-acc2-04d483052fc4']['gender']='male'
set Example.UserProfile['b5ac78c3-fd5c-40ca-acc2-04d483052fc4']['Tel']='13113668890'
set Example.UserProfile['b5ac78c3-fd5c-40ca-acc2-04d483052fc4']['Cellphone']='13113668890'

get Example.UserProfile['b5ac78c3-fd5c-40ca-acc2-04d483052fc4']

```

### Category

```
set Example.Category['85c1acb3-dc81-4626-aea9-c153dc80e74f']['uuid'] = '85c1acb3-dc81-4626-aea9-c153dc80e74f'
set Example.Category['85c1acb3-dc81-4626-aea9-c153dc80e74f']['name'] = '中国'
set Example.Category['85c1acb3-dc81-4626-aea9-c153dc80e74f']['description'] = '中华人民共和国'

set Example.Category['002f7fd4-455a-4f16-9cc8-38a43f9d285c']['uuid'] = '002f7fd4-455a-4f16-9cc8-38a43f9d285c'
set Example.Category['002f7fd4-455a-4f16-9cc8-38a43f9d285c']['name'] = '广东'
set Example.Category['002f7fd4-455a-4f16-9cc8-38a43f9d285c']['description'] = '广东省'
set Example.Category['002f7fd4-455a-4f16-9cc8-38a43f9d285c']['parent_uuid'] = '85c1acb3-dc81-4626-aea9-c153dc80e74f'

get Example.Category['002f7fd4-455a-4f16-9cc8-38a43f9d285c']

```

### Article

```
set Example.Article['862f0f17-a697-49b3-9bca-68b0cfc873ec']['uuid'] = '862f0f17-a697-49b3-9bca-68b0cfc873ec'
set Example.Article['862f0f17-a697-49b3-9bca-68b0cfc873ec']['title'] = '文章标题'
set Example.Article['862f0f17-a697-49b3-9bca-68b0cfc873ec']['content'] = '文章内容'
set Example.Article['862f0f17-a697-49b3-9bca-68b0cfc873ec']['author'] = 'Neo'
set Example.Article['862f0f17-a697-49b3-9bca-68b0cfc873ec']['datetime'] = '2010-5-10 12:00:00'

get Example.Article['862f0f17-a697-49b3-9bca-68b0cfc873ec']

```

### Product and ProductAttribute

Product data

```
set Example.Product['b12e97e1-63b4-4042-a3f2-da60005ec081']['uuid'] = 'b12e97e1-63b4-4042-a3f2-da60005ec081'
set Example.Product['b12e97e1-63b4-4042-a3f2-da60005ec081']['name'] = 'Dell Optiplex 780'
set Example.Product['b12e97e1-63b4-4042-a3f2-da60005ec081']['description'] = 'Dell Computer'
set Example.Product['b12e97e1-63b4-4042-a3f2-da60005ec081']['price'] = '5000'
set Example.Product['b12e97e1-63b4-4042-a3f2-da60005ec081']['image'] = '/www/images/dell780.jpg'
set Example.Product['b12e97e1-63b4-4042-a3f2-da60005ec081']['category_uuid'] = 'b12e97e1-63b4-4042-a3f2-da60005ec081'

get Example.Product['b12e97e1-63b4-4042-a3f2-da60005ec081']

```

product attribute

```
set Example.ProductAttribute['b12e97e1-63b4-4042-a3f2-da60005ec081']['color']['box'] = 'silver'
set Example.ProductAttribute['b12e97e1-63b4-4042-a3f2-da60005ec081']['color']['display'] = 'black'
set Example.ProductAttribute['b12e97e1-63b4-4042-a3f2-da60005ec081']['monitor']['size'] = '1440*900'
set Example.ProductAttribute['b12e97e1-63b4-4042-a3f2-da60005ec081']['monitor']['power'] = '12v'
set Example.ProductAttribute['b12e97e1-63b4-4042-a3f2-da60005ec081']['parameter']['process'] = 'Intel(R) Core(TM)2 Duo CPU E7500 @ 2.93Ghz'
set Example.ProductAttribute['b12e97e1-63b4-4042-a3f2-da60005ec081']['parameter']['memory'] = '2GB'
set Example.ProductAttribute['b12e97e1-63b4-4042-a3f2-da60005ec081']['parameter']['harddisk'] = '360GB'
set Example.ProductAttribute['b12e97e1-63b4-4042-a3f2-da60005ec081']['parameter']['disc'] = 'DVD RW'
set Example.ProductAttribute['b12e97e1-63b4-4042-a3f2-da60005ec081']['software']['os'] = 'Windows 7'
set Example.ProductAttribute['b12e97e1-63b4-4042-a3f2-da60005ec081']['software']['compress'] = '7zip'
set Example.ProductAttribute['b12e97e1-63b4-4042-a3f2-da60005ec081']['software']['media'] = 'Kmplay'
set Example.ProductAttribute['b12e97e1-63b4-4042-a3f2-da60005ec081']['software']['game'] = 'mine'

get Example.ProductAttribute['b12e97e1-63b4-4042-a3f2-da60005ec081']

```

### Address

```
set Example.Address['b5ac78c3-fd5c-40ca-acc2-04d483052fc4']['home']['street']='Longhua'
set Example.Address['b5ac78c3-fd5c-40ca-acc2-04d483052fc4']['home']['city']='Shenzhen'
set Example.Address['b5ac78c3-fd5c-40ca-acc2-04d483052fc4']['home']['zip']='518000'

set Example.Address['b5ac78c3-fd5c-40ca-acc2-04d483052fc4']['work']['street']='CheGongMiao'
set Example.Address['b5ac78c3-fd5c-40ca-acc2-04d483052fc4']['work']['city']='Shenzhen'
set Example.Address['b5ac78c3-fd5c-40ca-acc2-04d483052fc4']['work']['zip']='51800'

get Example.Address['b5ac78c3-fd5c-40ca-acc2-04d483052fc4']

```

### 练习

```
division
 id
 name
 country_id

department
 id
 name
 up_id
 path

division_has_department

employee
 id
 ename
 name
 sex
 age
 department_id

devices
 name
 sn

devices_attribute

employee_has_devices
 employee_id
 devices_id

```

## 第 10 章 数据库与缓存

## 什么是数据库缓存？

这里讲的缓存是数据库本身的缓存，并不是外部缓存例如 Redis/Memcache 等等。

数据库的数据分为冷数据和热数据库，通俗的讲冷数据是存储在磁盘上不经常查询的数据；而热数据是频繁查询的数据，这部分数据会被缓存到内存中。

## 为什么缓存数据呢？

因为频繁查询相同结果集的数据时，每次到磁盘上查找数据是非常耗时的，所以数据库将频繁查询且返回相同结果集的数据放到内存中，可以减少磁盘访问操作。

## 什么时候使用数据库缓存

频繁访问且返回相同结果集的情况下使用缓存。

偶尔查询一次且间隔时间较长的情况下不要使用缓存。

尺寸较大的结果集不建议使用缓存，因为数据太大太大，缓存不足以存储，会导致频繁载入与销毁，命中率低。

通常数据库默认情况是开启缓存的，也就是说正常的 select 查询，如果符合缓存规则就会经过缓存。

当一条 SQL 查询时如果结果集在内存中称作“命中”

## 涉及缓存的地方有哪些

数据库本身，查看数据库缓存状态

数据库应用程序接口（ODBC、JDBC......）

## 谁来控制数据库缓存

通常 DBA 只能控制数据库缓存是否开启，分配多少内存给缓存使用，过期销毁时间，以及策略等等.

上面我已经说过，通常数据库默认都开启缓存，所以更多的时候我们的操作是禁用缓存。这就需要开发人员来通过特定的 SQL 操作来控制数据库缓存。

## 怎么控制数据库缓存

以 MySQL 为例

```

mysql> show variables like '%query_cache%'; 
+------------------------------+---------+
| Variable_name                | Value   |
+------------------------------+---------+
| have_query_cache             | YES     |
| query_cache_limit            | 1048576 |
| query_cache_min_res_unit     | 4096    |
| query_cache_size             | 1048576 |
| query_cache_type             | OFF     |
| query_cache_wlock_invalidate | OFF     |
+------------------------------+---------+
6 rows in set (0.04 sec)		

```

编辑 my.cnf 文件，加入配置项 query_cache_type=1 然后重启 mysql 服务

```

mysql> show variables like '%query_cache%'; 
+------------------------------+---------+
| Variable_name                | Value   |
+------------------------------+---------+
| have_query_cache             | YES     |
| query_cache_limit            | 1048576 |
| query_cache_min_res_unit     | 4096    |
| query_cache_size             | 1048576 |
| query_cache_type             | ON      |
| query_cache_wlock_invalidate | OFF     |
+------------------------------+---------+
6 rows in set (0.00 sec)		

```

query_cache_type | ON 表示缓存已经开启。

### SQL_CACHE 缓存

默认情况 select 查询操作只要符合数据库缓存规则那么结果集就会被缓存，如果你的数据库没有开启缓存，请参考下面

```

set session query_cache_type=on;

flush tables;
show status like 'qcache_q%';
select sql_cache * from member where id=1;
show status like 'qcache_q%';
select sql_cache * from member where id=1;
show status like 'qcache_q%';

```

例 10.1. 演示 SQL_CACHE

```

mysql> flush tables;
Query OK, 0 rows affected (0.00 sec)

mysql> show status like 'qcache_q%';
+-------------------------+-------+
| Variable_name           | Value |
+-------------------------+-------+
| Qcache_queries_in_cache | 0     |
+-------------------------+-------+
1 row in set (0.00 sec)

mysql> select sql_cache * from member where id=1;
+----+-----+---------------------+------------+--------+-------+------+---------+------+--------+--------+
| id | age | ctime               | ip_address | mobile | mtime | name | picture | sex  | status | wechat |
+----+-----+---------------------+------------+--------+-------+------+---------+------+--------+--------+
|  1 |   1 | 2017-08-24 17:05:43 | 1          | NULL   | NULL  | 1    | 1       | 1    | Enable | NULL   |
+----+-----+---------------------+------------+--------+-------+------+---------+------+--------+--------+
1 row in set (0.00 sec)

mysql> show status like 'qcache_q%';
+-------------------------+-------+
| Variable_name           | Value |
+-------------------------+-------+
| Qcache_queries_in_cache | 1     |
+-------------------------+-------+
1 row in set (0.01 sec)

mysql> select sql_cache * from member where id=1;
+----+-----+---------------------+------------+--------+-------+------+---------+------+--------+--------+
| id | age | ctime               | ip_address | mobile | mtime | name | picture | sex  | status | wechat |
+----+-----+---------------------+------------+--------+-------+------+---------+------+--------+--------+
|  1 |   1 | 2017-08-24 17:05:43 | 1          | NULL   | NULL  | 1    | 1       | 1    | Enable | NULL   |
+----+-----+---------------------+------------+--------+-------+------+---------+------+--------+--------+
1 row in set (0.00 sec)

mysql> show status like 'qcache_q%';
+-------------------------+-------+
| Variable_name           | Value |
+-------------------------+-------+
| Qcache_queries_in_cache | 1     |
+-------------------------+-------+
1 row in set (0.01 sec)

```

我们可以看到 Qcache_queries_in_cache 值由 0 转为 1 表示缓存已经生效。

### 禁止缓存 SQL_NO_CACHE

这里我们主要讲怎样禁止缓存，使查询出的结果集不进入缓存。

```
SELECT SQL_NO_CACHE * FROM table where id=xxxx			

```

下面的用法比较安全，切换到其他数据库也能正常工作

```
SELECT /*!40001 SQL_NO_CACHE */ * FROM table			

```

```
set session query_cache_type=on;

flush tables;
show status like 'qcache_q%';
select sql_no_cache * from member where id=1;
show status like 'qcache_q%';
select sql_no_cache * from member where id=1;
show status like 'qcache_q%';						

```

例 10.2. 演示 SQL_NO_CACHE

```

mysql> flush tables;
Query OK, 0 rows affected (0.00 sec)

mysql> show status like 'qcache_q%';
+-------------------------+-------+
| Variable_name           | Value |
+-------------------------+-------+
| Qcache_queries_in_cache | 0     |
+-------------------------+-------+
1 row in set (0.00 sec)

mysql> select sql_no_cache * from member where id=1;
+----+-----+---------------------+------------+--------+-------+------+---------+------+--------+--------+
| id | age | ctime               | ip_address | mobile | mtime | name | picture | sex  | status | wechat |
+----+-----+---------------------+------------+--------+-------+------+---------+------+--------+--------+
|  1 |   1 | 2017-08-24 17:05:43 | 1          | NULL   | NULL  | 1    | 1       | 1    | Enable | NULL   |
+----+-----+---------------------+------------+--------+-------+------+---------+------+--------+--------+
1 row in set (0.00 sec)

mysql> show status like 'qcache_q%';
+-------------------------+-------+
| Variable_name           | Value |
+-------------------------+-------+
| Qcache_queries_in_cache | 0     |
+-------------------------+-------+
1 row in set (0.00 sec)

mysql> select sql_no_cache * from member where id=1;
+----+-----+---------------------+------------+--------+-------+------+---------+------+--------+--------+
| id | age | ctime               | ip_address | mobile | mtime | name | picture | sex  | status | wechat |
+----+-----+---------------------+------------+--------+-------+------+---------+------+--------+--------+
|  1 |   1 | 2017-08-24 17:05:43 | 1          | NULL   | NULL  | 1    | 1       | 1    | Enable | NULL   |
+----+-----+---------------------+------------+--------+-------+------+---------+------+--------+--------+
1 row in set (0.00 sec)

mysql> show status like 'qcache_q%';
+-------------------------+-------+
| Variable_name           | Value |
+-------------------------+-------+
| Qcache_queries_in_cache | 0     |
+-------------------------+-------+
1 row in set (0.00 sec)

```

使用 sql_no_cache 查询 Qcache_queries_in_cache 值始终是 0

### 关闭缓存 set session query_cache_type=off

我们使用 set session query_cache_type=off 可以关闭本次查询缓存。

```
set session query_cache_type=off;

flush tables;
show status like 'qcache_q%';
select sql_cache * from member where id=1;
show status like 'qcache_q%';
select sql_cache * from member where id=1;
show status like 'qcache_q%';						

```

例 10.3. 演示 query_cache_type=off 关闭查询缓存

```

mysql> set session query_cache_type=off;
Query OK, 0 rows affected (0.00 sec)

mysql> 
mysql> flush tables;
Query OK, 0 rows affected (0.00 sec)

mysql> show status like 'qcache_q%';
+-------------------------+-------+
| Variable_name           | Value |
+-------------------------+-------+
| Qcache_queries_in_cache | 0     |
+-------------------------+-------+
1 row in set (0.00 sec)

mysql> select sql_cache * from member where id=1;
+----+-----+---------------------+------------+--------+-------+------+---------+------+--------+--------+
| id | age | ctime               | ip_address | mobile | mtime | name | picture | sex  | status | wechat |
+----+-----+---------------------+------------+--------+-------+------+---------+------+--------+--------+
|  1 |   1 | 2017-08-24 17:05:43 | 1          | NULL   | NULL  | 1    | 1       | 1    | Enable | NULL   |
+----+-----+---------------------+------------+--------+-------+------+---------+------+--------+--------+
1 row in set (0.00 sec)

mysql> show status like 'qcache_q%';
+-------------------------+-------+
| Variable_name           | Value |
+-------------------------+-------+
| Qcache_queries_in_cache | 0     |
+-------------------------+-------+
1 row in set (0.00 sec)

mysql> select sql_cache * from member where id=1;
+----+-----+---------------------+------------+--------+-------+------+---------+------+--------+--------+
| id | age | ctime               | ip_address | mobile | mtime | name | picture | sex  | status | wechat |
+----+-----+---------------------+------------+--------+-------+------+---------+------+--------+--------+
|  1 |   1 | 2017-08-24 17:05:43 | 1          | NULL   | NULL  | 1    | 1       | 1    | Enable | NULL   |
+----+-----+---------------------+------------+--------+-------+------+---------+------+--------+--------+
1 row in set (0.00 sec)

mysql> show status like 'qcache_q%';
+-------------------------+-------+
| Variable_name           | Value |
+-------------------------+-------+
| Qcache_queries_in_cache | 0     |
+-------------------------+-------+
1 row in set (0.00 sec)

```

## 第 11 章 数据属性

常用属性参考

## 布尔状态

布尔状态

```
Enabled, Disabled, Yes, No		

```

```
Rejected, Approved
Allowed, Denied

```

```
Success,Failure

'Succeed','Failed'

```

## 流状态

```
Canceled, Canceled Reversal
Complete
Expired, Extension
Chargeback, Refunded, Reversed
Shipped, Voided	

```

状态流

```
New -> Pending -> Processing -> Processed	

```

使用案例

```
'New' -> 'Unassigned' -> 'Assigned' -> 'Pending' -> 'Invalid'

```

混合使用案例

```
'New' -> 'Processing' -> 'Canceled' / 'Completed' / 'Failed' / Deleted

```

```
'New','Processing','Cancelling','Canceled','Completed','Failed','Hidden'		

```

## 商品属性

这些属性很常见，你可以将它插入到你的数据库中，方便日后使用

### 鞋

```
鞋
	20
	20.5
	21
	21.5
	22
	22.5
	23
	24
	25
	25.5
	26
	26.5
	27
	27.5
	28
	28.5
	29
	30
	31
	32
	33
	33.5
	34
	34.5
	35
	35.5
	36
	36.5
	36 2/3
	37
	37 1/3
	37.5
	38
	38
	38.5
	38 2/3
	39
	39 1/3
	39.5
	40
	40.5
	40 2/3
	41
	41 1/3
	41.5
	42
	42.5
	42 2/3
	43
	43 1/3
	43.5
	44
	44.5
	44 2/3
	45
	45 1/3
	45.5
	46
	46.5
	46 2/3
	47
	47 1/3
	47.5
	48
	48.5
	49
	49.5
	50
	51
	52    	

```

### 裤子

```
	26
	26.5
	27
	27.5
	28
	28.5
	29
	30
	31
	32
	33
	33.5
	34
	34.5
	35
	35.5
	36    	

```

### 服装

```
	42
	44
	46
	48
	50    	

```

```
其他服装
	F
	XF
	XXS
	XS
	S
	M
	L
	XL
	XXL
	XXXL
	XXXXL
	XXXXXL

```

### 内衣

```
	 70A
	 70B
	 70C
	 70D
	 72A
	 72S
	 74A
	 74S
	 75A
	 75B
	 75C
	 75D
	 75E
	 76A
	 76S
	 78A
	 78S
	 80A
	 80B
	 80C
	 80D
	 80E
	 80S
	 82A
	 82S
	 83S
	 84A
	 84B
	 84S
	 85A
	 85B
	 85C
	 85D
	 85E
	 86S
	 88A
	 88B
	 88S
	 90B
	 90C
	 90D
	 90S
	 92S
	 94S
	 96S
	 98S    	

```

文胸

```
	A 罩
	B 罩
	C 罩
	D 罩

```

### 隐形眼镜

```
	100
	125
	150
	175
	200
	225
	250
	275
	300
	325
	350
	375
	400
	425
	450
	475
	500
	550
	600
	650
	700
	750
	800
	850
	900
	950
	1000    	

```

### 戒指

```
	11
	12
	13
	14
	15
	16
	17
	18
	19

```

## 手机号码分配

```

移动：134，135，136，137，138，139，150，151，152，157，158，159，187，188，147，182

联通：130，131，132，155，156，185，186，140

电信：180，159，133，153 ，189

中国电信发布中国 3G 号码段，中国联通 185，186；中国移动 188，187；中国电信 189，180 共 6 个号段。目前，3G 业务专属的 180-189 号段已基本分配给各运营商使用，其中 180、189 分配给中国电信，187、188 归中国移动使用，185、186 属于新联通。

中国移动拥有号码段为：139、138、137、136、135、134、159、158、157（3G）、151、150、188（3G）、187（3G）；13 个号段

中国联通拥有号码段为：130、131、132、156（3G）、186（3G）、185（3G）；6 个号段

中国电信拥有号码段为：133、153、189（3G）、180（3G）；4 个号码段

```

## 身份证

```
LOAD DATA LOW_PRIORITY LOCAL INFILE 'C:\\Users\\neo\\Desktop\\id.csv' INTO TABLE `identity_card` FIELDS TERMINATED BY ',' LINES TERMINATED BY '\r\n' (`number`, `zone`);

CREATE TEMPORARY TABLE tmp1 select * from identity_card where number like '%0000';
update  identity_card id,tmp1 t1 set id.province=t1.zone where left(id.number,2) = left(t1.number,2);
CREATE TEMPORARY TABLE tmp2 select * from identity_card where number like '%00';
update  identity_card id,tmp2 t1 set id.city=t1.zone where left(id.number,4) = left(t1.number,4);
update  identity_card set county=zone;
update  identity_card set county='' where county=city;
update  identity_card set city='' where province=city;

CREATE TABLE `identity_location` (
	`prefix` VARCHAR(32) NOT NULL COMMENT '身份证号码段',
	`province` VARCHAR(50) NULL DEFAULT NULL COMMENT '省份',
	`city` VARCHAR(50) NULL DEFAULT NULL COMMENT '城市',
	`county` VARCHAR(50) NULL DEFAULT NULL COMMENT '县',
	`status` ENUM('Y','N') NOT NULL DEFAULT 'N' COMMENT '状态',
	`mtime` TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '创建与修改时间',
	PRIMARY KEY (`prefix`)
)
COMMENT='identity card number'
COLLATE='utf8_general_ci'
ENGINE=InnoDB;

insert into identity_location(prefix,province,city,county) select md5(number) as prefix,province,city,county from identity_card;		

```

```
110000,北京市
110100,市辖区
110101,东城区
110102,西城区
110105,朝阳区
110106,丰台区
110107,石景山区
110108,海淀区
110109,门头沟区
110111,房山区
110112,通州区
110113,顺义区
110114,昌平区
110115,大兴区
110116,怀柔区
110117,平谷区
110200,县
110228,密云县
110229,延庆县
120000,天津市
120100,市辖区
120101,和平区
120102,河东区
120103,河西区
120104,南开区
120105,河北区
120106,红桥区
120110,东丽区
120111,西青区
120112,津南区
120113,北辰区
120114,武清区
120115,宝坻区
120116,滨海新区
120200,县
120221,宁河县
120223,静海县
120225,蓟县
130000,河北省
130100,石家庄市
130101,市辖区
130102,长安区
130103,桥东区
130104,桥西区
130105,新华区
130107,井陉矿区
130108,裕华区
130121,井陉县
130123,正定县
130124,栾城县
130125,行唐县
130126,灵寿县
130127,高邑县
130128,深泽县
130129,赞皇县
130130,无极县
130131,平山县
130132,元氏县
130133,赵县
130181,辛集市
130182,藁城市
130183,晋州市
130184,新乐市
130185,鹿泉市
130200,唐山市
130201,市辖区
130202,路南区
130203,路北区
130204,古冶区
130205,开平区
130207,丰南区
130208,丰润区
130209,曹妃甸区
130223,滦县
130224,滦南县
130225,乐亭县
130227,迁西县
130229,玉田县
130281,遵化市
130283,迁安市
130300,秦皇岛市
130301,市辖区
130302,海港区
130303,山海关区
130304,北戴河区
130321,青龙满族自治县
130322,昌黎县
130323,抚宁县
130324,卢龙县
130400,邯郸市
130401,市辖区
130402,邯山区
130403,丛台区
130404,复兴区
130406,峰峰矿区
130421,邯郸县
130423,临漳县
130424,成安县
130425,大名县
130426,涉县
130427,磁县
130428,肥乡县
130429,永年县
130430,邱县
130431,鸡泽县
130432,广平县
130433,馆陶县
130434,魏县
130435,曲周县
130481,武安市
130500,邢台市
130501,市辖区
130502,桥东区
130503,桥西区
130521,邢台县
130522,临城县
130523,内丘县
130524,柏乡县
130525,隆尧县
130526,任县
130527,南和县
130528,宁晋县
130529,巨鹿县
130530,新河县
130531,广宗县
130532,平乡县
130533,威县
130534,清河县
130535,临西县
130581,南宫市
130582,沙河市
130600,保定市
130601,市辖区
130602,新市区
130603,北市区
130604,南市区
130621,满城县
130622,清苑县
130623,涞水县
130624,阜平县
130625,徐水县
130626,定兴县
130627,唐县
130628,高阳县
130629,容城县
130630,涞源县
130631,望都县
130632,安新县
130633,易县
130634,曲阳县
130635,蠡县
130636,顺平县
130637,博野县
130638,雄县
130681,涿州市
130682,定州市
130683,安国市
130684,高碑店市
130700,张家口市
130701,市辖区
130702,桥东区
130703,桥西区
130705,宣化区
130706,下花园区
130721,宣化县
130722,张北县
130723,康保县
130724,沽源县
130725,尚义县
130726,蔚县
130727,阳原县
130728,怀安县
130729,万全县
130730,怀来县
130731,涿鹿县
130732,赤城县
130733,崇礼县
130800,承德市
130801,市辖区
130802,双桥区
130803,双滦区
130804,鹰手营子矿区
130821,承德县
130822,兴隆县
130823,平泉县
130824,滦平县
130825,隆化县
130826,丰宁满族自治县
130827,宽城满族自治县
130828,围场满族蒙古族自治县
130900,沧州市
130901,市辖区
130902,新华区
130903,运河区
130921,沧县
130922,青县
130923,东光县
130924,海兴县
130925,盐山县
130926,肃宁县
130927,南皮县
130928,吴桥县
130929,献县
130930,孟村回族自治县
130981,泊头市
130982,任丘市
130983,黄骅市
130984,河间市
131000,廊坊市
131001,市辖区
131002,安次区
131003,广阳区
131022,固安县
131023,永清县
131024,香河县
131025,大城县
131026,文安县
131028,大厂回族自治县
131081,霸州市
131082,三河市
131100,衡水市
131101,市辖区
131102,桃城区
131121,枣强县
131122,武邑县
131123,武强县
131124,饶阳县
131125,安平县
131126,故城县
131127,景县
131128,阜城县
131181,冀州市
131182,深州市
140000,山西省
140100,太原市
140101,市辖区
140105,小店区
140106,迎泽区
140107,杏花岭区
140108,尖草坪区
140109,万柏林区
140110,晋源区
140121,清徐县
140122,阳曲县
140123,娄烦县
140181,古交市
140200,大同市
140201,市辖区
140202,城区
140203,矿区
140211,南郊区
140212,新荣区
140221,阳高县
140222,天镇县
140223,广灵县
140224,灵丘县
140225,浑源县
140226,左云县
140227,大同县
140300,阳泉市
140301,市辖区
140302,城区
140303,矿区
140311,郊区
140321,平定县
140322,盂县
140400,长治市
140401,市辖区
140402,城区
140411,郊区
140421,长治县
140423,襄垣县
140424,屯留县
140425,平顺县
140426,黎城县
140427,壶关县
140428,长子县
140429,武乡县
140430,沁县
140431,沁源县
140481,潞城市
140500,晋城市
140501,晋城市市辖区
140502,城区
140521,沁水县
140522,阳城县
140524,陵川县
140525,泽州县
140581,高平市
140600,朔州市
140601,市辖区
140602,朔城区
140603,平鲁区
140621,山阴县
140622,应县
140623,右玉县
140624,怀仁县
140700,晋中市
140701,市辖区
140702,榆次区
140721,榆社县
140722,左权县
140723,和顺县
140724,昔阳县
140725,寿阳县
140726,太谷县
140727,祁县
140728,平遥县
140729,灵石县
140781,介休市
140800,运城市
140801,市辖区
140802,盐湖区
140821,临猗县
140822,万荣县
140823,闻喜县
140824,稷山县
140825,新绛县
140826,绛县
140827,垣曲县
140828,夏县
140829,平陆县
140830,芮城县
140881,永济市
140882,河津市
140900,忻州市
140901,市辖区
140902,忻府区
140921,定襄县
140922,五台县
140923,代县
140924,繁峙县
140925,宁武县
140926,静乐县
140927,神池县
140928,五寨县
140929,岢岚县
140930,河曲县
140931,保德县
140932,偏关县
140981,原平市
141000,临汾市
141001,市辖区
141002,尧都区
141021,曲沃县
141022,翼城县
141023,襄汾县
141024,洪洞县
141025,古县
141026,安泽县
141027,浮山县
141028,吉县
141029,乡宁县
141030,大宁县
141031,隰县
141032,永和县
141033,蒲县
141034,汾西县
141081,侯马市
141082,霍州市
141100,吕梁市
141101,市辖区
141102,离石区
141121,文水县
141122,交城县
141123,兴县
141124,临县
141125,柳林县
141126,石楼县
141127,岚县
141128,方山县
141129,中阳县
141130,交口县
141181,孝义市
141182,汾阳市
150000,内蒙古自治区
150100,呼和浩特市
150101,市辖区
150102,新城区
150103,回民区
150104,玉泉区
150105,赛罕区
150121,土默特左旗
150122,托克托县
150123,和林格尔县
150124,清水河县
150125,武川县
150200,包头市
150201,市辖区
150202,东河区
150203,昆都仑区
150204,青山区
150205,石拐区
150206,白云鄂博矿区
150207,九原区
150221,土默特右旗
150222,固阳县
150223,达尔罕茂明安联合旗
150300,乌海市
150301,市辖区
150302,海勃湾区
150303,海南区
150304,乌达区
150400,赤峰市
150401,市辖区
150402,红山区
150403,元宝山区
150404,松山区
150421,阿鲁科尔沁旗
150422,巴林左旗
150423,巴林右旗
150424,林西县
150425,克什克腾旗
150426,翁牛特旗
150428,喀喇沁旗
150429,宁城县
150430,敖汉旗
150500,通辽市
150501,市辖区
150502,科尔沁区
150521,科尔沁左翼中旗
150522,科尔沁左翼后旗
150523,开鲁县
150524,库伦旗
150525,奈曼旗
150526,扎鲁特旗
150581,霍林郭勒市
150600,鄂尔多斯市
150601,市辖区
150602,东胜区
150621,达拉特旗
150622,准格尔旗
150623,鄂托克前旗
150624,鄂托克旗
150625,杭锦旗
150626,乌审旗
150627,伊金霍洛旗
150700,呼伦贝尔市
150701,市辖区
150702,海拉尔区
150721,阿荣旗
150722,莫力达瓦达斡尔族自治旗
150723,鄂伦春自治旗
150724,鄂温克族自治旗
150725,陈巴尔虎旗
150726,新巴尔虎左旗
150727,新巴尔虎右旗
150781,满洲里市
150782,牙克石市
150783,扎兰屯市
150784,额尔古纳市
150785,根河市
150800,巴彦淖尔市
150801,市辖区
150802,临河区
150821,五原县
150822,磴口县
150823,乌拉特前旗
150824,乌拉特中旗
150825,乌拉特后旗
150826,杭锦后旗
150900,乌兰察布市
150901,市辖区
150902,集宁区
150921,卓资县
150922,化德县
150923,商都县
150924,兴和县
150925,凉城县
150926,察哈尔右翼前旗
150927,察哈尔右翼中旗
150928,察哈尔右翼后旗
150929,四子王旗
150981,丰镇市
152200,兴安盟
152201,乌兰浩特市
152202,阿尔山市
152221,科尔沁右翼前旗
152222,科尔沁右翼中旗
152223,扎赉特旗
152224,突泉县
152500,锡林郭勒盟
152501,二连浩特市
152502,锡林浩特市
152522,阿巴嘎旗
152523,苏尼特左旗
152524,苏尼特右旗
152525,东乌珠穆沁旗
152526,西乌珠穆沁旗
152527,太仆寺旗
152528,镶黄旗
152529,正镶白旗
152530,正蓝旗
152531,多伦县
152900,阿拉善盟
152921,阿拉善左旗
152922,阿拉善右旗
152923,额济纳旗
210000,辽宁省
210100,沈阳市
210101,市辖区
210102,和平区
210103,沈河区
210104,大东区
210105,皇姑区
210106,铁西区
210111,苏家屯区
210112,东陵区
210113,沈北新区
210114,于洪区
210122,辽中县
210123,康平县
210124,法库县
210181,新民市
210200,大连市
210201,市辖区
210202,中山区
210203,西岗区
210204,沙河口区
210211,甘井子区
210212,旅顺口区
210213,金州区
210224,长海县
210281,瓦房店市
210282,普兰店市
210283,庄河市
210300,鞍山市
210301,市辖区
210302,铁东区
210303,铁西区
210304,立山区
210311,千山区
210321,台安县
210323,岫岩满族自治县
210381,海城市
210400,抚顺市
210401,市辖区
210402,新抚区
210403,东洲区
210404,望花区
210411,顺城区
210421,抚顺县
210422,新宾满族自治县
210423,清原满族自治县
210500,本溪市
210501,市辖区
210502,平山区
210503,溪湖区
210504,明山区
210505,南芬区
210521,本溪满族自治县
210522,桓仁满族自治县
210600,丹东市
210601,市辖区
210602,元宝区
210603,振兴区
210604,振安区
210624,宽甸满族自治县
210681,东港市
210682,凤城市
210700,锦州市
210701,市辖区
210702,古塔区
210703,凌河区
210711,太和区
210726,黑山县
210727,义县
210781,凌海市
210782,北镇市
210800,营口市
210801,市辖区
210802,站前区
210803,西市区
210804,鲅鱼圈区
210811,老边区
210881,盖州市
210882,大石桥市
210900,阜新市
210901,市辖区
210902,海州区
210903,新邱区
210904,太平区
210905,清河门区
210911,细河区
210921,阜新蒙古族自治县
210922,彰武县
211000,辽阳市
211001,市辖区
211002,白塔区
211003,文圣区
211004,宏伟区
211005,弓长岭区
211011,太子河区
211021,辽阳县
211081,灯塔市
211100,盘锦市
211101,市辖区
211102,双台子区
211103,兴隆台区
211121,大洼县
211122,盘山县
211200,铁岭市
211201,市辖区
211202,银州区
211204,清河区
211221,铁岭县
211223,西丰县
211224,昌图县
211281,调兵山市
211282,开原市
211300,朝阳市
211301,市辖区
211302,双塔区
211303,龙城区
211321,朝阳县
211322,建平县
211324,喀喇沁左翼蒙古族自治县
211381,北票市
211382,凌源市
211400,葫芦岛市
211401,市辖区
211402,连山区
211403,龙港区
211404,南票区
211421,绥中县
211422,建昌县
211481,兴城市
220000,吉林省
220100,长春市
220101,市辖区
220102,南关区
220103,宽城区
220104,朝阳区
220105,二道区
220106,绿园区
220112,双阳区
220122,农安县
220181,九台市
220182,榆树市
220183,德惠市
220200,吉林市
220201,市辖区
220202,昌邑区
220203,龙潭区
220204,船营区
220211,丰满区
220221,永吉县
220281,蛟河市
220282,桦甸市
220283,舒兰市
220284,磐石市
220300,四平市
220301,市辖区
220302,铁西区
220303,铁东区
220322,梨树县
220323,伊通满族自治县
220381,公主岭市
220382,双辽市
220400,辽源市
220401,市辖区
220402,龙山区
220403,西安区
220421,东丰县
220422,东辽县
220500,通化市
220501,市辖区
220502,东昌区
220503,二道江区
220521,通化县
220523,辉南县
220524,柳河县
220581,梅河口市
220582,集安市
220600,白山市
220601,市辖区
220602,浑江区
220605,江源区
220621,抚松县
220622,靖宇县
220623,长白朝鲜族自治县
220681,临江市
220700,松原市
220701,市辖区
220702,宁江区
220721,前郭尔罗斯蒙古族自治县
220722,长岭县
220723,乾安县
220724,扶余县
220800,白城市
220801,市辖区
220802,洮北区
220821,镇赉县
220822,通榆县
220881,洮南市
220882,大安市
222400,延边朝鲜族自治州
222401,延吉市
222402,图们市
222403,敦化市
222404,珲春市
222405,龙井市
222406,和龙市
222424,汪清县
222426,安图县
230000,黑龙江省
230100,哈尔滨市
230101,市辖区
230102,道里区
230103,南岗区
230104,道外区
230108,平房区
230109,松北区
230110,香坊区
230111,呼兰区
230112,阿城区
230123,依兰县
230124,方正县
230125,宾县
230126,巴彦县
230127,木兰县
230128,通河县
230129,延寿县
230182,双城市
230183,尚志市
230184,五常市
230200,齐齐哈尔市
230201,市辖区
230202,龙沙区
230203,建华区
230204,铁锋区
230205,昂昂溪区
230206,富拉尔基区
230207,碾子山区
230208,梅里斯达斡尔族区
230221,龙江县
230223,依安县
230224,泰来县
230225,甘南县
230227,富裕县
230229,克山县
230230,克东县
230231,拜泉县
230281,讷河市
230300,鸡西市
230301,市辖区
230302,鸡冠区
230303,恒山区
230304,滴道区
230305,梨树区
230306,城子河区
230307,麻山区
230321,鸡东县
230381,虎林市
230382,密山市
230400,鹤岗市
230401,市辖区
230402,向阳区
230403,工农区
230404,南山区
230405,兴安区
230406,东山区
230407,兴山区
230421,萝北县
230422,绥滨县
230500,双鸭山市
230501,市辖区
230502,尖山区
230503,岭东区
230505,四方台区
230506,宝山区
230521,集贤县
230522,友谊县
230523,宝清县
230524,饶河县
230600,大庆市
230601,市辖区
230602,萨尔图区
230603,龙凤区
230604,让胡路区
230605,红岗区
230606,大同区
230621,肇州县
230622,肇源县
230623,林甸县
230624,杜尔伯特蒙古族自治县
230700,伊春市
230701,市辖区
230702,伊春区
230703,南岔区
230704,友好区
230705,西林区
230706,翠峦区
230707,新青区
230708,美溪区
230709,金山屯区
230710,五营区
230711,乌马河区
230712,汤旺河区
230713,带岭区
230714,乌伊岭区
230715,红星区
230716,上甘岭区
230722,嘉荫县
230781,铁力市
230800,佳木斯市
230801,市辖区
230803,向阳区
230804,前进区
230805,东风区
230811,郊区
230822,桦南县
230826,桦川县
230828,汤原县
230833,抚远县
230881,同江市
230882,富锦市
230900,七台河市
230901,市辖区
230902,新兴区
230903,桃山区
230904,茄子河区
230921,勃利县
231000,牡丹江市
231001,市辖区
231002,东安区
231003,阳明区
231004,爱民区
231005,西安区
231024,东宁县
231025,林口县
231081,绥芬河市
231083,海林市
231084,宁安市
231085,穆棱市
231100,黑河市
231101,市辖区
231102,爱辉区
231121,嫩江县
231123,逊克县
231124,孙吴县
231181,北安市
231182,五大连池市
231200,绥化市
231201,市辖区
231202,北林区
231221,望奎县
231222,兰西县
231223,青冈县
231224,庆安县
231225,明水县
231226,绥棱县
231281,安达市
231282,肇东市
231283,海伦市
232700,大兴安岭地区
232721,呼玛县
232722,塔河县
232723,漠河县
310000,上海市
310100,市辖区
310101,黄浦区
310104,徐汇区
310105,长宁区
310106,静安区
310107,普陀区
310108,闸北区
310109,虹口区
310110,杨浦区
310112,闵行区
310113,宝山区
310114,嘉定区
310115,浦东新区
310116,金山区
310117,松江区
310118,青浦区
310120,奉贤区
310200,县
310230,崇明县
320000,江苏省
320100,南京市
320101,市辖区
320102,玄武区
320103,白下区
320104,秦淮区
320105,建邺区
320106,鼓楼区
320107,下关区
320111,浦口区
320113,栖霞区
320114,雨花台区
320115,江宁区
320116,六合区
320124,溧水县
320125,高淳县
320200,无锡市
320201,市辖区
320202,崇安区
320203,南长区
320204,北塘区
320205,锡山区
320206,惠山区
320211,滨湖区
320281,江阴市
320282,宜兴市
320300,徐州市
320301,市辖区
320302,鼓楼区
320303,云龙区
320305,贾汪区
320311,泉山区
320312,铜山区
320321,丰县
320322,沛县
320324,睢宁县
320381,新沂市
320382,邳州市
320400,常州市
320401,市辖区
320402,天宁区
320404,钟楼区
320405,戚墅堰区
320411,新北区
320412,武进区
320481,溧阳市
320482,金坛市
320500,苏州市
320501,市辖区
320505,虎丘区
320506,吴中区
320507,相城区
320508,姑苏区
320509,吴江区
320581,常熟市
320582,张家港市
320583,昆山市
320585,太仓市
320600,南通市
320601,市辖区
320602,崇川区
320611,港闸区
320612,通州区
320621,海安县
320623,如东县
320681,启东市
320682,如皋市
320684,海门市
320700,连云港市
320701,市辖区
320703,连云区
320705,新浦区
320706,海州区
320721,赣榆县
320722,东海县
320723,灌云县
320724,灌南县
320800,淮安市
320801,市辖区
320802,清河区
320803,淮安区
320804,淮阴区
320811,清浦区
320826,涟水县
320829,洪泽县
320830,盱眙县
320831,金湖县
320900,盐城市
320901,市辖区
320902,亭湖区
320903,盐都区
320921,响水县
320922,滨海县
320923,阜宁县
320924,射阳县
320925,建湖县
320981,东台市
320982,大丰市
321000,扬州市
321001,市辖区
321002,广陵区
321003,邗江区
321012,江都区
321023,宝应县
321081,仪征市
321084,高邮市
321100,镇江市
321101,市辖区
321102,京口区
321111,润州区
321112,丹徒区
321181,丹阳市
321182,扬中市
321183,句容市
321200,泰州市
321201,市辖区
321202,海陵区
321203,高港区
321281,兴化市
321282,靖江市
321283,泰兴市
321284,姜堰市
321300,宿迁市
321301,市辖区
321302,宿城区
321311,宿豫区
321322,沭阳县
321323,泗阳县
321324,泗洪县
330000,浙江省
330100,杭州市
330101,市辖区
330102,上城区
330103,下城区
330104,江干区
330105,拱墅区
330106,西湖区
330108,滨江区
330109,萧山区
330110,余杭区
330122,桐庐县
330127,淳安县
330182,建德市
330183,富阳市
330185,临安市
330200,宁波市
330201,市辖区
330203,海曙区
330204,江东区
330205,江北区
330206,北仑区
330211,镇海区
330212,鄞州区
330225,象山县
330226,宁海县
330281,余姚市
330282,慈溪市
330283,奉化市
330300,温州市
330301,市辖区
330302,鹿城区
330303,龙湾区
330304,瓯海区
330322,洞头县
330324,永嘉县
330326,平阳县
330327,苍南县
330328,文成县
330329,泰顺县
330381,瑞安市
330382,乐清市
330400,嘉兴市
330401,市辖区
330402,南湖区
330411,秀洲区
330421,嘉善县
330424,海盐县
330481,海宁市
330482,平湖市
330483,桐乡市
330500,湖州市
330501,市辖区
330502,吴兴区
330503,南浔区
330521,德清县
330522,长兴县
330523,安吉县
330600,绍兴市
330601,市辖区
330602,越城区
330621,绍兴县
330624,新昌县
330681,诸暨市
330682,上虞市
330683,嵊州市
330700,金华市
330701,市辖区
330702,婺城区
330703,金东区
330723,武义县
330726,浦江县
330727,磐安县
330781,兰溪市
330782,义乌市
330783,东阳市
330784,永康市
330800,衢州市
330801,市辖区
330802,柯城区
330803,衢江区
330822,常山县
330824,开化县
330825,龙游县
330881,江山市
330900,舟山市
330901,市辖区
330902,定海区
330903,普陀区
330921,岱山县
330922,嵊泗县
331000,台州市
331001,市辖区
331002,椒江区
331003,黄岩区
331004,路桥区
331021,玉环县
331022,三门县
331023,天台县
331024,仙居县
331081,温岭市
331082,临海市
331100,丽水市
331101,市辖区
331102,莲都区
331121,青田县
331122,缙云县
331123,遂昌县
331124,松阳县
331125,云和县
331126,庆元县
331127,景宁畲族自治县
331181,龙泉市
340000,安徽省
340100,合肥市
340101,市辖区
340102,瑶海区
340103,庐阳区
340104,蜀山区
340111,包河区
340121,长丰县
340122,肥东县
340123,肥西县
340124,庐江县
340181,巢湖市
340200,芜湖市
340201,市辖区
340202,镜湖区
340203,弋江区
340207,鸠江区
340208,三山区
340221,芜湖县
340222,繁昌县
340223,南陵县
340225,无为县
340300,蚌埠市
340301,市辖区
340302,龙子湖区
340303,蚌山区
340304,禹会区
340311,淮上区
340321,怀远县
340322,五河县
340323,固镇县
340400,淮南市
340401,市辖区
340402,大通区
340403,田家庵区
340404,谢家集区
340405,八公山区
340406,潘集区
340421,凤台县
340500,马鞍山市
340501,市辖区
340503,花山区
340504,雨山区
340506,博望区
340521,当涂县
340522,含山县
340523,和县
340600,淮北市
340601,市辖区
340602,杜集区
340603,相山区
340604,烈山区
340621,濉溪县
340700,铜陵市
340701,市辖区
340702,铜官山区
340703,狮子山区
340711,郊区
340721,铜陵县
340800,安庆市
340801,市辖区
340802,迎江区
340803,大观区
340811,宜秀区
340822,怀宁县
340823,枞阳县
340824,潜山县
340825,太湖县
340826,宿松县
340827,望江县
340828,岳西县
340881,桐城市
341000,黄山市
341001,市辖区
341002,屯溪区
341003,黄山区
341004,徽州区
341021,歙县
341022,休宁县
341023,黟县
341024,祁门县
341100,滁州市
341101,市辖区
341102,琅琊区
341103,南谯区
341122,来安县
341124,全椒县
341125,定远县
341126,凤阳县
341181,天长市
341182,明光市
341200,阜阳市
341201,市辖区
341202,颍州区
341203,颍东区
341204,颍泉区
341221,临泉县
341222,太和县
341225,阜南县
341226,颍上县
341282,界首市
341300,宿州市
341301,市辖区
341302,埇桥区
341321,砀山县
341322,萧县
341323,灵璧县
341324,泗县
341500,六安市
341501,市辖区
341502,金安区
341503,裕安区
341521,寿县
341522,霍邱县
341523,舒城县
341524,金寨县
341525,霍山县
341600,亳州市
341601,市辖区
341602,谯城区
341621,涡阳县
341622,蒙城县
341623,利辛县
341700,池州市
341701,市辖区
341702,贵池区
341721,东至县
341722,石台县
341723,青阳县
341800,宣城市
341801,市辖区
341802,宣州区
341821,郎溪县
341822,广德县
341823,泾县
341824,绩溪县
341825,旌德县
341881,宁国市
350000,福建省
350100,福州市
350101,市辖区
350102,鼓楼区
350103,台江区
350104,仓山区
350105,马尾区
350111,晋安区
350121,闽侯县
350122,连江县
350123,罗源县
350124,闽清县
350125,永泰县
350128,平潭县
350181,福清市
350182,长乐市
350200,厦门市
350201,市辖区
350203,思明区
350205,海沧区
350206,湖里区
350211,集美区
350212,同安区
350213,翔安区
350300,莆田市
350301,市辖区
350302,城厢区
350303,涵江区
350304,荔城区
350305,秀屿区
350322,仙游县
350400,三明市
350401,市辖区
350402,梅列区
350403,三元区
350421,明溪县
350423,清流县
350424,宁化县
350425,大田县
350426,尤溪县
350427,沙县
350428,将乐县
350429,泰宁县
350430,建宁县
350481,永安市
350500,泉州市
350501,市辖区
350502,鲤城区
350503,丰泽区
350504,洛江区
350505,泉港区
350521,惠安县
350524,安溪县
350525,永春县
350526,德化县
350527,金门县
350581,石狮市
350582,晋江市
350583,南安市
350600,漳州市
350601,市辖区
350602,芗城区
350603,龙文区
350622,云霄县
350623,漳浦县
350624,诏安县
350625,长泰县
350626,东山县
350627,南靖县
350628,平和县
350629,华安县
350681,龙海市
350700,南平市
350701,市辖区
350702,延平区
350721,顺昌县
350722,浦城县
350723,光泽县
350724,松溪县
350725,政和县
350781,邵武市
350782,武夷山市
350783,建瓯市
350784,建阳市
350800,龙岩市
350801,市辖区
350802,新罗区
350821,长汀县
350822,永定县
350823,上杭县
350824,武平县
350825,连城县
350881,漳平市
350900,宁德市
350901,市辖区
350902,蕉城区
350921,霞浦县
350922,古田县
350923,屏南县
350924,寿宁县
350925,周宁县
350926,柘荣县
350981,福安市
350982,福鼎市
360000,江西省
360100,南昌市
360101,市辖区
360102,东湖区
360103,西湖区
360104,青云谱区
360105,湾里区
360111,青山湖区
360121,南昌县
360122,新建县
360123,安义县
360124,进贤县
360200,景德镇市
360201,市辖区
360202,昌江区
360203,珠山区
360222,浮梁县
360281,乐平市
360300,萍乡市
360301,市辖区
360302,安源区
360313,湘东区
360321,莲花县
360322,上栗县
360323,芦溪县
360400,九江市
360401,市辖区
360402,庐山区
360403,浔阳区
360421,九江县
360423,武宁县
360424,修水县
360425,永修县
360426,德安县
360427,星子县
360428,都昌县
360429,湖口县
360430,彭泽县
360481,瑞昌市
360482,共青城市
360500,新余市
360501,市辖区
360502,渝水区
360521,分宜县
360600,鹰潭市
360601,市辖区
360602,月湖区
360622,余江县
360681,贵溪市
360700,赣州市
360701,市辖区
360702,章贡区
360721,赣县
360722,信丰县
360723,大余县
360724,上犹县
360725,崇义县
360726,安远县
360727,龙南县
360728,定南县
360729,全南县
360730,宁都县
360731,于都县
360732,兴国县
360733,会昌县
360734,寻乌县
360735,石城县
360781,瑞金市
360782,南康市
360800,吉安市
360801,市辖区
360802,吉州区
360803,青原区
360821,吉安县
360822,吉水县
360823,峡江县
360824,新干县
360825,永丰县
360826,泰和县
360827,遂川县
360828,万安县
360829,安福县
360830,永新县
360881,井冈山市
360900,宜春市
360901,市辖区
360902,袁州区
360921,奉新县
360922,万载县
360923,上高县
360924,宜丰县
360925,靖安县
360926,铜鼓县
360981,丰城市
360982,樟树市
360983,高安市
361000,抚州市
361001,市辖区
361002,临川区
361021,南城县
361022,黎川县
361023,南丰县
361024,崇仁县
361025,乐安县
361026,宜黄县
361027,金溪县
361028,资溪县
361029,东乡县
361030,广昌县
361100,上饶市
361101,市辖区
361102,信州区
361121,上饶县
361122,广丰县
361123,玉山县
361124,铅山县
361125,横峰县
361126,弋阳县
361127,余干县
361128,鄱阳县
361129,万年县
361130,婺源县
361181,德兴市
370000,山东省
370100,济南市
370101,市辖区
370102,历下区
370103,市中区
370104,槐荫区
370105,天桥区
370112,历城区
370113,长清区
370124,平阴县
370125,济阳县
370126,商河县
370181,章丘市
370200,青岛市
370201,市辖区
370202,市南区
370203,市北区
370205,四方区
370211,黄岛区
370212,崂山区
370213,李沧区
370214,城阳区
370281,胶州市
370282,即墨市
370283,平度市
370284,胶南市
370285,莱西市
370300,淄博市
370301,市辖区
370302,淄川区
370303,张店区
370304,博山区
370305,临淄区
370306,周村区
370321,桓台县
370322,高青县
370323,沂源县
370400,枣庄市
370401,市辖区
370402,市中区
370403,薛城区
370404,峄城区
370405,台儿庄区
370406,山亭区
370481,滕州市
370500,东营市
370501,市辖区
370502,东营区
370503,河口区
370521,垦利县
370522,利津县
370523,广饶县
370600,烟台市
370601,市辖区
370602,芝罘区
370611,福山区
370612,牟平区
370613,莱山区
370634,长岛县
370681,龙口市
370682,莱阳市
370683,莱州市
370684,蓬莱市
370685,招远市
370686,栖霞市
370687,海阳市
370700,潍坊市
370701,市辖区
370702,潍城区
370703,寒亭区
370704,坊子区
370705,奎文区
370724,临朐县
370725,昌乐县
370781,青州市
370782,诸城市
370783,寿光市
370784,安丘市
370785,高密市
370786,昌邑市
370800,济宁市
370801,市辖区
370802,市中区
370811,任城区
370826,微山县
370827,鱼台县
370828,金乡县
370829,嘉祥县
370830,汶上县
370831,泗水县
370832,梁山县
370881,曲阜市
370882,兖州市
370883,邹城市
370900,泰安市
370901,市辖区
370902,泰山区
370911,岱岳区
370921,宁阳县
370923,东平县
370982,新泰市
370983,肥城市
371000,威海市
371001,市辖区
371002,环翠区
371081,文登市
371082,荣成市
371083,乳山市
371100,日照市
371101,市辖区
371102,东港区
371103,岚山区
371121,五莲县
371122,莒县
371200,莱芜市
371201,市辖区
371202,莱城区
371203,钢城区
371300,临沂市
371301,市辖区
371302,兰山区
371311,罗庄区
371312,河东区
371321,沂南县
371322,郯城县
371323,沂水县
371324,苍山县
371325,费县
371326,平邑县
371327,莒南县
371328,蒙阴县
371329,临沭县
371400,德州市
371401,市辖区
371402,德城区
371421,陵县
371422,宁津县
371423,庆云县
371424,临邑县
371425,齐河县
371426,平原县
371427,夏津县
371428,武城县
371481,乐陵市
371482,禹城市
371500,聊城市
371501,市辖区
371502,东昌府区
371521,阳谷县
371522,莘县
371523,茌平县
371524,东阿县
371525,冠县
371526,高唐县
371581,临清市
371600,滨州市
371601,市辖区
371602,滨城区
371621,惠民县
371622,阳信县
371623,无棣县
371624,沾化县
371625,博兴县
371626,邹平县
371700,菏泽市
371701,市辖区
371702,牡丹区
371721,曹县
371722,单县
371723,成武县
371724,巨野县
371725,郓城县
371726,鄄城县
371727,定陶县
371728,东明县
410000,河南省
410100,郑州市
410101,市辖区
410102,中原区
410103,二七区
410104,管城回族区
410105,金水区
410106,上街区
410108,惠济区
410122,中牟县
410181,巩义市
410182,荥阳市
410183,新密市
410184,新郑市
410185,登封市
410200,开封市
410201,市辖区
410202,龙亭区
410203,顺河回族区
410204,鼓楼区
410205,禹王台区
410211,金明区
410221,杞县
410222,通许县
410223,尉氏县
410224,开封县
410225,兰考县
410300,洛阳市
410301,市辖区
410302,老城区
410303,西工区
410304,瀍河回族区
410305,涧西区
410306,吉利区
410311,洛龙区
410322,孟津县
410323,新安县
410324,栾川县
410325,嵩县
410326,汝阳县
410327,宜阳县
410328,洛宁县
410329,伊川县
410381,偃师市
410400,平顶山市
410401,市辖区
410402,新华区
410403,卫东区
410404,石龙区
410411,湛河区
410421,宝丰县
410422,叶县
410423,鲁山县
410425,郏县
410481,舞钢市
410482,汝州市
410500,安阳市
410501,市辖区
410502,文峰区
410503,北关区
410505,殷都区
410506,龙安区
410522,安阳县
410523,汤阴县
410526,滑县
410527,内黄县
410581,林州市
410600,鹤壁市
410601,市辖区
410602,鹤山区
410603,山城区
410611,淇滨区
410621,浚县
410622,淇县
410700,新乡市
410701,市辖区
410702,红旗区
410703,卫滨区
410704,凤泉区
410711,牧野区
410721,新乡县
410724,获嘉县
410725,原阳县
410726,延津县
410727,封丘县
410728,长垣县
410781,卫辉市
410782,辉县市
410800,焦作市
410801,市辖区
410802,解放区
410803,中站区
410804,马村区
410811,山阳区
410821,修武县
410822,博爱县
410823,武陟县
410825,温县
410882,沁阳市
410883,孟州市
410900,濮阳市
410901,市辖区
410902,华龙区
410922,清丰县
410923,南乐县
410926,范县
410927,台前县
410928,濮阳县
411000,许昌市
411001,市辖区
411002,魏都区
411023,许昌县
411024,鄢陵县
411025,襄城县
411081,禹州市
411082,长葛市
411100,漯河市
411101,市辖区
411102,源汇区
411103,郾城区
411104,召陵区
411121,舞阳县
411122,临颍县
411200,三门峡市
411201,市辖区
411202,湖滨区
411221,渑池县
411222,陕县
411224,卢氏县
411281,义马市
411282,灵宝市
411300,南阳市
411301,市辖区
411302,宛城区
411303,卧龙区
411321,南召县
411322,方城县
411323,西峡县
411324,镇平县
411325,内乡县
411326,淅川县
411327,社旗县
411328,唐河县
411329,新野县
411330,桐柏县
411381,邓州市
411400,商丘市
411401,市辖区
411402,梁园区
411403,睢阳区
411421,民权县
411422,睢县
411423,宁陵县
411424,柘城县
411425,虞城县
411426,夏邑县
411481,永城市
411500,信阳市
411501,市辖区
411502,浉河区
411503,平桥区
411521,罗山县
411522,光山县
411523,新县
411524,商城县
411525,固始县
411526,潢川县
411527,淮滨县
411528,息县
411600,周口市
411601,市辖区
411602,川汇区
411621,扶沟县
411622,西华县
411623,商水县
411624,沈丘县
411625,郸城县
411626,淮阳县
411627,太康县
411628,鹿邑县
411681,项城市
411700,驻马店市
411701,市辖区
411702,驿城区
411721,西平县
411722,上蔡县
411723,平舆县
411724,正阳县
411725,确山县
411726,泌阳县
411727,汝南县
411728,遂平县
411729,新蔡县
419000,省直辖县级行政区划
419001,济源市
420000,湖北省
420100,武汉市
420101,市辖区
420102,江岸区
420103,江汉区
420104,硚口区
420105,汉阳区
420106,武昌区
420107,青山区
420111,洪山区
420112,东西湖区
420113,汉南区
420114,蔡甸区
420115,江夏区
420116,黄陂区
420117,新洲区
420200,黄石市
420201,市辖区
420202,黄石港区
420203,西塞山区
420204,下陆区
420205,铁山区
420222,阳新县
420281,大冶市
420300,十堰市
420301,市辖区
420302,茅箭区
420303,张湾区
420321,郧县
420322,郧西县
420323,竹山县
420324,竹溪县
420325,房县
420381,丹江口市
420500,宜昌市
420501,市辖区
420502,西陵区
420503,伍家岗区
420504,点军区
420505,猇亭区
420506,夷陵区
420525,远安县
420526,兴山县
420527,秭归县
420528,长阳土家族自治县
420529,五峰土家族自治县
420581,宜都市
420582,当阳市
420583,枝江市
420600,襄阳市
420601,市辖区
420602,襄城区
420606,樊城区
420607,襄州区
420624,南漳县
420625,谷城县
420626,保康县
420682,老河口市
420683,枣阳市
420684,宜城市
420700,鄂州市
420701,市辖区
420702,梁子湖区
420703,华容区
420704,鄂城区
420800,荆门市
420801,市辖区
420802,东宝区
420804,掇刀区
420821,京山县
420822,沙洋县
420881,钟祥市
420900,孝感市
420901,市辖区
420902,孝南区
420921,孝昌县
420922,大悟县
420923,云梦县
420981,应城市
420982,安陆市
420984,汉川市
421000,荆州市
421001,市辖区
421002,沙市区
421003,荆州区
421022,公安县
421023,监利县
421024,江陵县
421081,石首市
421083,洪湖市
421087,松滋市
421100,黄冈市
421101,市辖区
421102,黄州区
421121,团风县
421122,红安县
421123,罗田县
421124,英山县
421125,浠水县
421126,蕲春县
421127,黄梅县
421181,麻城市
421182,武穴市
421200,咸宁市
421201,市辖区
421202,咸安区
421221,嘉鱼县
421222,通城县
421223,崇阳县
421224,通山县
421281,赤壁市
421300,随州市
421301,市辖区
421303,曾都区
421321,随县
421381,广水市
422800,恩施土家族苗族自治州
422801,恩施市
422802,利川市
422822,建始县
422823,巴东县
422825,宣恩县
422826,咸丰县
422827,来凤县
422828,鹤峰县
429000,省直辖县级行政区划
429004,仙桃市
429005,潜江市
429006,天门市
429021,神农架林区
430000,湖南省
430100,长沙市
430101,市辖区
430102,芙蓉区
430103,天心区
430104,岳麓区
430105,开福区
430111,雨花区
430112,望城区
430121,长沙县
430124,宁乡县
430181,浏阳市
430200,株洲市
430201,市辖区
430202,荷塘区
430203,芦淞区
430204,石峰区
430211,天元区
430221,株洲县
430223,攸县
430224,茶陵县
430225,炎陵县
430281,醴陵市
430300,湘潭市
430301,市辖区
430302,雨湖区
430304,岳塘区
430321,湘潭县
430381,湘乡市
430382,韶山市
430400,衡阳市
430401,市辖区
430405,珠晖区
430406,雁峰区
430407,石鼓区
430408,蒸湘区
430412,南岳区
430421,衡阳县
430422,衡南县
430423,衡山县
430424,衡东县
430426,祁东县
430481,耒阳市
430482,常宁市
430500,邵阳市
430501,市辖区
430502,双清区
430503,大祥区
430511,北塔区
430521,邵东县
430522,新邵县
430523,邵阳县
430524,隆回县
430525,洞口县
430527,绥宁县
430528,新宁县
430529,城步苗族自治县
430581,武冈市
430600,岳阳市
430601,市辖区
430602,岳阳楼区
430603,云溪区
430611,君山区
430621,岳阳县
430623,华容县
430624,湘阴县
430626,平江县
430681,汨罗市
430682,临湘市
430700,常德市
430701,市辖区
430702,武陵区
430703,鼎城区
430721,安乡县
430722,汉寿县
430723,澧县
430724,临澧县
430725,桃源县
430726,石门县
430781,津市市
430800,张家界市
430801,市辖区
430802,永定区
430811,武陵源区
430821,慈利县
430822,桑植县
430900,益阳市
430901,市辖区
430902,资阳区
430903,赫山区
430921,南县
430922,桃江县
430923,安化县
430981,沅江市
431000,郴州市
431001,市辖区
431002,北湖区
431003,苏仙区
431021,桂阳县
431022,宜章县
431023,永兴县
431024,嘉禾县
431025,临武县
431026,汝城县
431027,桂东县
431028,安仁县
431081,资兴市
431100,永州市
431101,市辖区
431102,零陵区
431103,冷水滩区
431121,祁阳县
431122,东安县
431123,双牌县
431124,道县
431125,江永县
431126,宁远县
431127,蓝山县
431128,新田县
431129,江华瑶族自治县
431200,怀化市
431201,市辖区
431202,鹤城区
431221,中方县
431222,沅陵县
431223,辰溪县
431224,溆浦县
431225,会同县
431226,麻阳苗族自治县
431227,新晃侗族自治县
431228,芷江侗族自治县
431229,靖州苗族侗族自治县
431230,通道侗族自治县
431281,洪江市
431300,娄底市
431301,市辖区
431302,娄星区
431321,双峰县
431322,新化县
431381,冷水江市
431382,涟源市
433100,湘西土家族苗族自治州
433101,吉首市
433122,泸溪县
433123,凤凰县
433124,花垣县
433125,保靖县
433126,古丈县
433127,永顺县
433130,龙山县
440000,广东省
440100,广州市
440101,市辖区
440103,荔湾区
440104,越秀区
440105,海珠区
440106,天河区
440111,白云区
440112,黄埔区
440113,番禺区
440114,花都区
440115,南沙区
440116,萝岗区
440183,增城市
440184,从化市
440200,韶关市
440201,市辖区
440203,武江区
440204,浈江区
440205,曲江区
440222,始兴县
440224,仁化县
440229,翁源县
440232,乳源瑶族自治县
440233,新丰县
440281,乐昌市
440282,南雄市
440300,深圳市
440301,市辖区
440303,罗湖区
440304,福田区
440305,南山区
440306,宝安区
440307,龙岗区
440308,盐田区
440400,珠海市
440401,市辖区
440402,香洲区
440403,斗门区
440404,金湾区
440500,汕头市
440501,市辖区
440507,龙湖区
440511,金平区
440512,濠江区
440513,潮阳区
440514,潮南区
440515,澄海区
440523,南澳县
440600,佛山市
440601,市辖区
440604,禅城区
440605,南海区
440606,顺德区
440607,三水区
440608,高明区
440700,江门市
440701,市辖区
440703,蓬江区
440704,江海区
440705,新会区
440781,台山市
440783,开平市
440784,鹤山市
440785,恩平市
440800,湛江市
440801,市辖区
440802,赤坎区
440803,霞山区
440804,坡头区
440811,麻章区
440823,遂溪县
440825,徐闻县
440881,廉江市
440882,雷州市
440883,吴川市
440900,茂名市
440901,市辖区
440902,茂南区
440903,茂港区
440923,电白县
440981,高州市
440982,化州市
440983,信宜市
441200,肇庆市
441201,市辖区
441202,端州区
441203,鼎湖区
441223,广宁县
441224,怀集县
441225,封开县
441226,德庆县
441283,高要市
441284,四会市
441300,惠州市
441301,市辖区
441302,惠城区
441303,惠阳区
441322,博罗县
441323,惠东县
441324,龙门县
441400,梅州市
441401,市辖区
441402,梅江区
441421,梅县
441422,大埔县
441423,丰顺县
441424,五华县
441426,平远县
441427,蕉岭县
441481,兴宁市
441500,汕尾市
441501,市辖区
441502,城区
441521,海丰县
441523,陆河县
441581,陆丰市
441600,河源市
441601,市辖区
441602,源城区
441621,紫金县
441622,龙川县
441623,连平县
441624,和平县
441625,东源县
441700,阳江市
441701,市辖区
441702,江城区
441721,阳西县
441723,阳东县
441781,阳春市
441800,清远市
441801,市辖区
441802,清城区
441821,佛冈县
441823,阳山县
441825,连山壮族瑶族自治县
441826,连南瑶族自治县
441827,清新县
441881,英德市
441882,连州市
441900,东莞市
442000,中山市
445100,潮州市
445101,市辖区
445102,湘桥区
445121,潮安县
445122,饶平县
445200,揭阳市
445201,市辖区
445202,榕城区
445221,揭东县
445222,揭西县
445224,惠来县
445281,普宁市
445300,云浮市
445301,市辖区
445302,云城区
445321,新兴县
445322,郁南县
445323,云安县
445381,罗定市
450000,广西壮族自治区
450100,南宁市
450101,市辖区
450102,兴宁区
450103,青秀区
450105,江南区
450107,西乡塘区
450108,良庆区
450109,邕宁区
450122,武鸣县
450123,隆安县
450124,马山县
450125,上林县
450126,宾阳县
450127,横县
450200,柳州市
450201,市辖区
450202,城中区
450203,鱼峰区
450204,柳南区
450205,柳北区
450221,柳江县
450222,柳城县
450223,鹿寨县
450224,融安县
450225,融水苗族自治县
450226,三江侗族自治县
450300,桂林市
450301,市辖区
450302,秀峰区
450303,叠彩区
450304,象山区
450305,七星区
450311,雁山区
450321,阳朔县
450322,临桂县
450323,灵川县
450324,全州县
450325,兴安县
450326,永福县
450327,灌阳县
450328,龙胜各族自治县
450329,资源县
450330,平乐县
450331,荔浦县
450332,恭城瑶族自治县
450400,梧州市
450401,市辖区
450403,万秀区
450404,蝶山区
450405,长洲区
450421,苍梧县
450422,藤县
450423,蒙山县
450481,岑溪市
450500,北海市
450501,市辖区
450502,海城区
450503,银海区
450512,铁山港区
450521,合浦县
450600,防城港市
450601,市辖区
450602,港口区
450603,防城区
450621,上思县
450681,东兴市
450700,钦州市
450701,市辖区
450702,钦南区
450703,钦北区
450721,灵山县
450722,浦北县
450800,贵港市
450801,市辖区
450802,港北区
450803,港南区
450804,覃塘区
450821,平南县
450881,桂平市
450900,玉林市
450901,市辖区
450902,玉州区
450921,容县
450922,陆川县
450923,博白县
450924,兴业县
450981,北流市
451000,百色市
451001,市辖区
451002,右江区
451021,田阳县
451022,田东县
451023,平果县
451024,德保县
451025,靖西县
451026,那坡县
451027,凌云县
451028,乐业县
451029,田林县
451030,西林县
451031,隆林各族自治县
451100,贺州市
451101,市辖区
451102,八步区
451121,昭平县
451122,钟山县
451123,富川瑶族自治县
451200,河池市
451201,市辖区
451202,金城江区
451221,南丹县
451222,天峨县
451223,凤山县
451224,东兰县
451225,罗城仫佬族自治县
451226,环江毛南族自治县
451227,巴马瑶族自治县
451228,都安瑶族自治县
451229,大化瑶族自治县
451281,宜州市
451300,来宾市
451301,市辖区
451302,兴宾区
451321,忻城县
451322,象州县
451323,武宣县
451324,金秀瑶族自治县
451381,合山市
451400,崇左市
451401,市辖区
451402,江洲区
451421,扶绥县
451422,宁明县
451423,龙州县
451424,大新县
451425,天等县
451481,凭祥市
460000,海南省
460100,海口市
460101,市辖区
460105,秀英区
460106,龙华区
460107,琼山区
460108,美兰区
460200,三亚市
460201,市辖区
460300,三沙市
460321,西沙群岛
460322,南沙群岛
460323,中沙群岛的岛礁及其海域
469000,省直辖县级行政区划
469001,五指山市
469002,琼海市
469003,儋州市
469005,文昌市
469006,万宁市
469007,东方市
469021,定安县
469022,屯昌县
469023,澄迈县
469024,临高县
469025,白沙黎族自治县
469026,昌江黎族自治县
469027,乐东黎族自治县
469028,陵水黎族自治县
469029,保亭黎族苗族自治县
469030,琼中黎族苗族自治县
500000,重庆市
500100,市辖区
500101,万州区
500102,涪陵区
500103,渝中区
500104,大渡口区
500105,江北区
500106,沙坪坝区
500107,九龙坡区
500108,南岸区
500109,北碚区
500110,綦江区
500111,大足区
500112,渝北区
500113,巴南区
500114,黔江区
500115,长寿区
500116,江津区
500117,合川区
500118,永川区
500119,南川区
500200,县
500223,潼南县
500224,铜梁县
500226,荣昌县
500227,璧山县
500228,梁平县
500229,城口县
500230,丰都县
500231,垫江县
500232,武隆县
500233,忠县
500234,开县
500235,云阳县
500236,奉节县
500237,巫山县
500238,巫溪县
500240,石柱土家族自治县
500241,秀山土家族苗族自治县
500242,酉阳土家族苗族自治县
500243,彭水苗族土家族自治县
510000,四川省
510100,成都市
510101,市辖区
510104,锦江区
510105,青羊区
510106,金牛区
510107,武侯区
510108,成华区
510112,龙泉驿区
510113,青白江区
510114,新都区
510115,温江区
510121,金堂县
510122,双流县
510124,郫县
510129,大邑县
510131,蒲江县
510132,新津县
510181,都江堰市
510182,彭州市
510183,邛崃市
510184,崇州市
510300,自贡市
510301,市辖区
510302,自流井区
510303,贡井区
510304,大安区
510311,沿滩区
510321,荣县
510322,富顺县
510400,攀枝花市
510401,市辖区
510402,东区
510403,西区
510411,仁和区
510421,米易县
510422,盐边县
510500,泸州市
510501,市辖区
510502,江阳区
510503,纳溪区
510504,龙马潭区
510521,泸县
510522,合江县
510524,叙永县
510525,古蔺县
510600,德阳市
510601,市辖区
510603,旌阳区
510623,中江县
510626,罗江县
510681,广汉市
510682,什邡市
510683,绵竹市
510700,绵阳市
510701,市辖区
510703,涪城区
510704,游仙区
510722,三台县
510723,盐亭县
510724,安县
510725,梓潼县
510726,北川羌族自治县
510727,平武县
510781,江油市
510800,广元市
510801,市辖区
510802,利州区
510811,元坝区
510812,朝天区
510821,旺苍县
510822,青川县
510823,剑阁县
510824,苍溪县
510900,遂宁市
510901,市辖区
510903,船山区
510904,安居区
510921,蓬溪县
510922,射洪县
510923,大英县
511000,内江市
511001,市辖区
511002,市中区
511011,东兴区
511024,威远县
511025,资中县
511028,隆昌县
511100,乐山市
511101,市辖区
511102,市中区
511111,沙湾区
511112,五通桥区
511113,金口河区
511123,犍为县
511124,井研县
511126,夹江县
511129,沐川县
511132,峨边彝族自治县
511133,马边彝族自治县
511181,峨眉山市
511300,南充市
511301,市辖区
511302,顺庆区
511303,高坪区
511304,嘉陵区
511321,南部县
511322,营山县
511323,蓬安县
511324,仪陇县
511325,西充县
511381,阆中市
511400,眉山市
511401,市辖区
511402,东坡区
511421,仁寿县
511422,彭山县
511423,洪雅县
511424,丹棱县
511425,青神县
511500,宜宾市
511501,市辖区
511502,翠屏区
511503,南溪区
511521,宜宾县
511523,江安县
511524,长宁县
511525,高县
511526,珙县
511527,筠连县
511528,兴文县
511529,屏山县
511600,广安市
511601,市辖区
511602,广安区
511621,岳池县
511622,武胜县
511623,邻水县
511681,华蓥市
511700,达州市
511701,市辖区
511702,通川区
511721,达县
511722,宣汉县
511723,开江县
511724,大竹县
511725,渠县
511781,万源市
511800,雅安市
511801,市辖区
511802,雨城区
511803,名山区
511822,荥经县
511823,汉源县
511824,石棉县
511825,天全县
511826,芦山县
511827,宝兴县
511900,巴中市
511901,市辖区
511902,巴州区
511921,通江县
511922,南江县
511923,平昌县
512000,资阳市
512001,市辖区
512002,雁江区
512021,安岳县
512022,乐至县
512081,简阳市
513200,阿坝藏族羌族自治州
513221,汶川县
513222,理县
513223,茂县
513224,松潘县
513225,九寨沟县
513226,金川县
513227,小金县
513228,黑水县
513229,马尔康县
513230,壤塘县
513231,阿坝县
513232,若尔盖县
513233,红原县
513300,甘孜藏族自治州
513321,康定县
513322,泸定县
513323,丹巴县
513324,九龙县
513325,雅江县
513326,道孚县
513327,炉霍县
513328,甘孜县
513329,新龙县
513330,德格县
513331,白玉县
513332,石渠县
513333,色达县
513334,理塘县
513335,巴塘县
513336,乡城县
513337,稻城县
513338,得荣县
513400,凉山彝族自治州
513401,西昌市
513422,木里藏族自治县
513423,盐源县
513424,德昌县
513425,会理县
513426,会东县
513427,宁南县
513428,普格县
513429,布拖县
513430,金阳县
513431,昭觉县
513432,喜德县
513433,冕宁县
513434,越西县
513435,甘洛县
513436,美姑县
513437,雷波县
520000,贵州省
520100,贵阳市
520101,市辖区
520102,南明区
520103,云岩区
520111,花溪区
520112,乌当区
520113,白云区
520114,小河区
520121,开阳县
520122,息烽县
520123,修文县
520181,清镇市
520200,六盘水市
520201,钟山区
520203,六枝特区
520221,水城县
520222,盘县
520300,遵义市
520301,市辖区
520302,红花岗区
520303,汇川区
520321,遵义县
520322,桐梓县
520323,绥阳县
520324,正安县
520325,道真仡佬族苗族自治县
520326,务川仡佬族苗族自治县
520327,凤冈县
520328,湄潭县
520329,余庆县
520330,习水县
520381,赤水市
520382,仁怀市
520400,安顺市
520401,市辖区
520402,西秀区
520421,平坝县
520422,普定县
520423,镇宁布依族苗族自治县
520424,关岭布依族苗族自治县
520425,紫云苗族布依族自治县
520500,毕节市
520502,七星关区
520521,大方县
520522,黔西县
520523,金沙县
520524,织金县
520525,纳雍县
520526,威宁彝族回族苗族自治县
520527,赫章县
520600,铜仁市
520602,碧江区
520603,万山区
520621,江口县
520622,玉屏侗族自治县
520623,石阡县
520624,思南县
520625,印江土家族苗族自治县
520626,德江县
520627,沿河土家族自治县
520628,松桃苗族自治县
522300,黔西南布依族苗族自治州
522301,兴义市
522322,兴仁县
522323,普安县
522324,晴隆县
522325,贞丰县
522326,望谟县
522327,册亨县
522328,安龙县
522600,黔东南苗族侗族自治州
522601,凯里市
522622,黄平县
522623,施秉县
522624,三穗县
522625,镇远县
522626,岑巩县
522627,天柱县
522628,锦屏县
522629,剑河县
522630,台江县
522631,黎平县
522632,榕江县
522633,从江县
522634,雷山县
522635,麻江县
522636,丹寨县
522700,黔南布依族苗族自治州
522701,都匀市
522702,福泉市
522722,荔波县
522723,贵定县
522725,瓮安县
522726,独山县
522727,平塘县
522728,罗甸县
522729,长顺县
522730,龙里县
522731,惠水县
522732,三都水族自治县
530000,云南省
530100,昆明市
530101,市辖区
530102,五华区
530103,盘龙区
530111,官渡区
530112,西山区
530113,东川区
530114,呈贡区
530122,晋宁县
530124,富民县
530125,宜良县
530126,石林彝族自治县
530127,嵩明县
530128,禄劝彝族苗族自治县
530129,寻甸回族彝族自治县
530181,安宁市
530300,曲靖市
530301,市辖区
530302,麒麟区
530321,马龙县
530322,陆良县
530323,师宗县
530324,罗平县
530325,富源县
530326,会泽县
530328,沾益县
530381,宣威市
530400,玉溪市
530402,红塔区
530421,江川县
530422,澄江县
530423,通海县
530424,华宁县
530425,易门县
530426,峨山彝族自治县
530427,新平彝族傣族自治县
530428,元江哈尼族彝族傣族自治县
530500,保山市
530501,市辖区
530502,隆阳区
530521,施甸县
530522,腾冲县
530523,龙陵县
530524,昌宁县
530600,昭通市
530601,市辖区
530602,昭阳区
530621,鲁甸县
530622,巧家县
530623,盐津县
530624,大关县
530625,永善县
530626,绥江县
530627,镇雄县
530628,彝良县
530629,威信县
530630,水富县
530700,丽江市
530701,市辖区
530702,古城区
530721,玉龙纳西族自治县
530722,永胜县
530723,华坪县
530724,宁蒗彝族自治县
530800,普洱市
530801,市辖区
530802,思茅区
530821,宁洱哈尼族彝族自治县
530822,墨江哈尼族自治县
530823,景东彝族自治县
530824,景谷傣族彝族自治县
530825,镇沅彝族哈尼族拉祜族自治县
530826,江城哈尼族彝族自治县
530827,孟连傣族拉祜族佤族自治县
530828,澜沧拉祜族自治县
530829,西盟佤族自治县
530900,临沧市
530901,市辖区
530902,临翔区
530921,凤庆县
530922,云县
530923,永德县
530924,镇康县
530925,双江拉祜族佤族布朗族傣族自治县
530926,耿马傣族佤族自治县
530927,沧源佤族自治县
532300,楚雄彝族自治州
532301,楚雄市
532322,双柏县
532323,牟定县
532324,南华县
532325,姚安县
532326,大姚县
532327,永仁县
532328,元谋县
532329,武定县
532331,禄丰县
532500,红河哈尼族彝族自治州
532501,个旧市
532502,开远市
532503,蒙自市
532523,屏边苗族自治县
532524,建水县
532525,石屏县
532526,弥勒县
532527,泸西县
532528,元阳县
532529,红河县
532530,金平苗族瑶族傣族自治县
532531,绿春县
532532,河口瑶族自治县
532600,文山壮族苗族自治州
532601,文山市
532622,砚山县
532623,西畴县
532624,麻栗坡县
532625,马关县
532626,丘北县
532627,广南县
532628,富宁县
532800,西双版纳傣族自治州
532801,景洪市
532822,勐海县
532823,勐腊县
532900,大理白族自治州
532901,大理市
532922,漾濞彝族自治县
532923,祥云县
532924,宾川县
532925,弥渡县
532926,南涧彝族自治县
532927,巍山彝族回族自治县
532928,永平县
532929,云龙县
532930,洱源县
532931,剑川县
532932,鹤庆县
533100,德宏傣族景颇族自治州
533102,瑞丽市
533103,芒市
533122,梁河县
533123,盈江县
533124,陇川县
533300,怒江傈僳族自治州
533321,泸水县
533323,福贡县
533324,贡山独龙族怒族自治县
533325,兰坪白族普米族自治县
533400,迪庆藏族自治州
533421,香格里拉县
533422,德钦县
533423,维西傈僳族自治县
540000,西藏自治区
540100,拉萨市
540101,市辖区
540102,城关区
540121,林周县
540122,当雄县
540123,尼木县
540124,曲水县
540125,堆龙德庆县
540126,达孜县
540127,墨竹工卡县
542100,昌都地区
542121,昌都县
542122,江达县
542123,贡觉县
542124,类乌齐县
542125,丁青县
542126,察雅县
542127,八宿县
542128,左贡县
542129,芒康县
542132,洛隆县
542133,边坝县
542200,山南地区
542221,乃东县
542222,扎囊县
542223,贡嘎县
542224,桑日县
542225,琼结县
542226,曲松县
542227,措美县
542228,洛扎县
542229,加查县
542231,隆子县
542232,错那县
542233,浪卡子县
542300,日喀则地区
542301,日喀则市
542322,南木林县
542323,江孜县
542324,定日县
542325,萨迦县
542326,拉孜县
542327,昂仁县
542328,谢通门县
542329,白朗县
542330,仁布县
542331,康马县
542332,定结县
542333,仲巴县
542334,亚东县
542335,吉隆县
542336,聂拉木县
542337,萨嘎县
542338,岗巴县
542400,那曲地区
542421,那曲县
542422,嘉黎县
542423,比如县
542424,聂荣县
542425,安多县
542426,申扎县
542427,索县
542428,班戈县
542429,巴青县
542430,尼玛县
542500,阿里地区
542521,普兰县
542522,札达县
542523,噶尔县
542524,日土县
542525,革吉县
542526,改则县
542527,措勤县
542600,林芝地区
542621,林芝县
542622,工布江达县
542623,米林县
542624,墨脱县
542625,波密县
542626,察隅县
542627,朗县
610000,陕西省
610100,西安市
610101,市辖区
610102,新城区
610103,碑林区
610104,莲湖区
610111,灞桥区
610112,未央区
610113,雁塔区
610114,阎良区
610115,临潼区
610116,长安区
610122,蓝田县
610124,周至县
610125,户县
610126,高陵县
610200,铜川市
610201,市辖区
610202,王益区
610203,印台区
610204,耀州区
610222,宜君县
610300,宝鸡市
610301,市辖区
610302,渭滨区
610303,金台区
610304,陈仓区
610322,凤翔县
610323,岐山县
610324,扶风县
610326,眉县
610327,陇县
610328,千阳县
610329,麟游县
610330,凤县
610331,太白县
610400,咸阳市
610401,市辖区
610402,秦都区
610403,杨陵区
610404,渭城区
610422,三原县
610423,泾阳县
610424,乾县
610425,礼泉县
610426,永寿县
610427,彬县
610428,长武县
610429,旬邑县
610430,淳化县
610431,武功县
610481,兴平市
610500,渭南市
610501,市辖区
610502,临渭区
610521,华县
610522,潼关县
610523,大荔县
610524,合阳县
610525,澄城县
610526,蒲城县
610527,白水县
610528,富平县
610581,韩城市
610582,华阴市
610600,延安市
610601,市辖区
610602,宝塔区
610621,延长县
610622,延川县
610623,子长县
610624,安塞县
610625,志丹县
610626,吴起县
610627,甘泉县
610628,富县
610629,洛川县
610630,宜川县
610631,黄龙县
610632,黄陵县
610700,汉中市
610701,市辖区
610702,汉台区
610721,南郑县
610722,城固县
610723,洋县
610724,西乡县
610725,勉县
610726,宁强县
610727,略阳县
610728,镇巴县
610729,留坝县
610730,佛坪县
610800,榆林市
610801,市辖区
610802,榆阳区
610821,神木县
610822,府谷县
610823,横山县
610824,靖边县
610825,定边县
610826,绥德县
610827,米脂县
610828,佳县
610829,吴堡县
610830,清涧县
610831,子洲县
610900,安康市
610901,市辖区
610902,汉滨区
610921,汉阴县
610922,石泉县
610923,宁陕县
610924,紫阳县
610925,岚皋县
610926,平利县
610927,镇坪县
610928,旬阳县
610929,白河县
611000,商洛市
611001,市辖区
611002,商州区
611021,洛南县
611022,丹凤县
611023,商南县
611024,山阳县
611025,镇安县
611026,柞水县
620000,甘肃省
620100,兰州市
620101,市辖区
620102,城关区
620103,七里河区
620104,西固区
620105,安宁区
620111,红古区
620121,永登县
620122,皋兰县
620123,榆中县
620200,嘉峪关市
620201,市辖区
620300,金昌市
620301,市辖区
620302,金川区
620321,永昌县
620400,白银市
620401,市辖区
620402,白银区
620403,平川区
620421,靖远县
620422,会宁县
620423,景泰县
620500,天水市
620501,市辖区
620502,秦州区
620503,麦积区
620521,清水县
620522,秦安县
620523,甘谷县
620524,武山县
620525,张家川回族自治县
620600,武威市
620601,市辖区
620602,凉州区
620621,民勤县
620622,古浪县
620623,天祝藏族自治县
620700,张掖市
620701,市辖区
620702,甘州区
620721,肃南裕固族自治县
620722,民乐县
620723,临泽县
620724,高台县
620725,山丹县
620800,平凉市
620801,市辖区
620802,崆峒区
620821,泾川县
620822,灵台县
620823,崇信县
620824,华亭县
620825,庄浪县
620826,静宁县
620900,酒泉市
620901,市辖区
620902,肃州区
620921,金塔县
620922,瓜州县
620923,肃北蒙古族自治县
620924,阿克塞哈萨克族自治县
620981,玉门市
620982,敦煌市
621000,庆阳市
621001,市辖区
621002,西峰区
621021,庆城县
621022,环县
621023,华池县
621024,合水县
621025,正宁县
621026,宁县
621027,镇原县
621100,定西市
621101,市辖区
621102,安定区
621121,通渭县
621122,陇西县
621123,渭源县
621124,临洮县
621125,漳县
621126,岷县
621200,陇南市
621201,市辖区
621202,武都区
621221,成县
621222,文县
621223,宕昌县
621224,康县
621225,西和县
621226,礼县
621227,徽县
621228,两当县
622900,临夏回族自治州
622901,临夏市
622921,临夏县
622922,康乐县
622923,永靖县
622924,广河县
622925,和政县
622926,东乡族自治县
622927,积石山保安族东乡族撒拉族自治县
623000,甘南藏族自治州
623001,合作市
623021,临潭县
623022,卓尼县
623023,舟曲县
623024,迭部县
623025,玛曲县
623026,碌曲县
623027,夏河县
630000,青海省
630100,西宁市
630101,市辖区
630102,城东区
630103,城中区
630104,城西区
630105,城北区
630121,大通回族土族自治县
630122,湟中县
630123,湟源县
632100,海东地区
632121,平安县
632122,民和回族土族自治县
632123,乐都县
632126,互助土族自治县
632127,化隆回族自治县
632128,循化撒拉族自治县
632200,海北藏族自治州
632221,门源回族自治县
632222,祁连县
632223,海晏县
632224,刚察县
632300,黄南藏族自治州
632321,同仁县
632322,尖扎县
632323,泽库县
632324,河南蒙古族自治县
632500,海南藏族自治州
632521,共和县
632522,同德县
632523,贵德县
632524,兴海县
632525,贵南县
632600,果洛藏族自治州
632621,玛沁县
632622,班玛县
632623,甘德县
632624,达日县
632625,久治县
632626,玛多县
632700,玉树藏族自治州
632721,玉树县
632722,杂多县
632723,称多县
632724,治多县
632725,囊谦县
632726,曲麻莱县
632800,海西蒙古族藏族自治州
632801,格尔木市
632802,德令哈市
632821,乌兰县
632822,都兰县
632823,天峻县
640000,宁夏回族自治区
640100,银川市
640101,市辖区
640104,兴庆区
640105,西夏区
640106,金凤区
640121,永宁县
640122,贺兰县
640181,灵武市
640200,石嘴山市
640201,市辖区
640202,大武口区
640205,惠农区
640221,平罗县
640300,吴忠市
640301,市辖区
640302,利通区
640303,红寺堡区
640323,盐池县
640324,同心县
640381,青铜峡市
640400,固原市
640401,市辖区
640402,原州区
640422,西吉县
640423,隆德县
640424,泾源县
640425,彭阳县
640500,中卫市
640501,市辖区
640502,沙坡头区
640521,中宁县
640522,海原县
650000,新疆维吾尔自治区
650100,乌鲁木齐市
650101,市辖区
650102,天山区
650103,沙依巴克区
650104,新市区
650105,水磨沟区
650106,头屯河区
650107,达坂城区
650109,米东区
650121,乌鲁木齐县
650200,克拉玛依市
650201,市辖区
650202,独山子区
650203,克拉玛依区
650204,白碱滩区
650205,乌尔禾区
652100,吐鲁番地区
652101,吐鲁番市
652122,鄯善县
652123,托克逊县
652200,哈密地区
652201,哈密市
652222,巴里坤哈萨克自治县
652223,伊吾县
652300,昌吉回族自治州
652301,昌吉市
652302,阜康市
652323,呼图壁县
652324,玛纳斯县
652325,奇台县
652327,吉木萨尔县
652328,木垒哈萨克自治县
652700,博尔塔拉蒙古自治州
652701,博乐市
652722,精河县
652723,温泉县
652800,巴音郭楞蒙古自治州
652801,库尔勒市
652822,轮台县
652823,尉犁县
652824,若羌县
652825,且末县
652826,焉耆回族自治县
652827,和静县
652828,和硕县
652829,博湖县
652900,阿克苏地区
652901,阿克苏市
652922,温宿县
652923,库车县
652924,沙雅县
652925,新和县
652926,拜城县
652927,乌什县
652928,阿瓦提县
652929,柯坪县
653000,克孜勒苏柯尔克孜自治州
653001,阿图什市
653022,阿克陶县
653023,阿合奇县
653024,乌恰县
653100,喀什地区
653101,喀什市
653121,疏附县
653122,疏勒县
653123,英吉沙县
653124,泽普县
653125,莎车县
653126,叶城县
653127,麦盖提县
653128,岳普湖县
653129,伽师县
653130,巴楚县
653131,塔什库尔干塔吉克自治县
653200,和田地区
653201,和田市
653221,和田县
653222,墨玉县
653223,皮山县
653224,洛浦县
653225,策勒县
653226,于田县
653227,民丰县
654000,伊犁哈萨克自治州
654002,伊宁市
654003,奎屯市
654021,伊宁县
654022,察布查尔锡伯自治县
654023,霍城县
654024,巩留县
654025,新源县
654026,昭苏县
654027,特克斯县
654028,尼勒克县
654200,塔城地区
654201,塔城市
654202,乌苏市
654221,额敏县
654223,沙湾县
654224,托里县
654225,裕民县
654226,和布克赛尔蒙古自治县
654300,阿勒泰地区
654301,阿勒泰市
654321,布尔津县
654322,富蕴县
654323,福海县
654324,哈巴河县
654325,青河县
654326,吉木乃县
659000,自治区直辖县级行政区划
659001,石河子市
659002,阿拉尔市
659003,图木舒克市
659004,五家渠市
710000,台湾省
810000,香港特别行政区
820000,澳门特别行政区	

```

[身份证校验码](http://zh.wikipedia.org/wiki/%E4%B8%AD%E5%8D%8E%E4%BA%BA%E6%B0%91%E5%85%B1%E5%92%8C%E5%9B%BD%E5%85%AC%E6%B0%91%E8%BA%AB%E4%BB%BD%E5%8F%B7%E7%A0%81)

```
 将前面的身份证号码 17 位数分别乘以不同的系数。从第一位到第十七位的系数分别为：7 9 10 5 8 4 2 1 6 3 7 9 10 5 8 4 2 ；

将这 17 位数字和系数相乘的结果相加；

用加出来和除以 11，看余数是多少？；

余数只可能有 0 1 2 3 4 5 6 7 8 9 10 这 11 个数字。其分别对应的最后一位身份证的号码为 1 0 X 9 8 7 6 5 4 3 2； 		

```

## 第 12 章 Spring Data 最佳实践

## MySQL

ORM 的出现解决了程序猿学习数据库学历成本，也加快了开发的速度。程序猿无需再学习数据库定义语言 DDL 以及数据库客户端，也无需关注建表这些繁琐的工作，同时也降低了数据库结构变更管理中与 DBA 频繁沟通的成本。。

在过去的两年中我们采用 Spring Data JPA 定义数据库，访问数据库，积累了很多经验，最终我们发现使用 Spring Data 实体定义完全可以代替 DBA 的建模工作。

下面我们采用案例，一个一个讲解，各种数据库实体关系的定义。相关数据库建模知识请先阅读 [《Netkiller Architect 手札》](http://www.netkiller.cn/architect/index.html) 以及 [《Netkiller Spring 手札》](http://www.netkiller.cn/spring/index.html)

### 分类表

这是一个通用分类表，常见的父子关系加上 path 路径

```

 +-----------+
 | category  |
 |-----------|
 |id         | <---+
 |name       |     |
 |description|    1:n
 |status     |     |
 |pid        | o---+
 |path       |
 |status     |
 |ctime      |
 |mtime      |
 +-----------+

```

```

package cn.netkiller.domain;

import java.util.Date;
import java.util.Set;

import javax.persistence.CascadeType;
import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.FetchType;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.JoinColumn;
import javax.persistence.ManyToOne;
import javax.persistence.OneToMany;

import org.springframework.format.annotation.DateTimeFormat;

import com.fasterxml.jackson.annotation.JsonFormat;
import com.fasterxml.jackson.annotation.JsonIgnore;

@Entity
public class Category {
	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	@Column(name = "id", unique = true, nullable = false, insertable = true, updatable = false)
	public int id;
	public String name;
	public String description;
	public String path;

	@Column(columnDefinition = "enum('Enabled','Disabled') DEFAULT 'Enabled' COMMENT '状态'")
	public String status;

	@DateTimeFormat(pattern = "yyyy-MM-dd HH:mm:ss")
	@JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss", timezone = "GMT+8")
	@Column(columnDefinition = "TIMESTAMP DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间'")
	public Date ctime;

	@DateTimeFormat(pattern = "yyyy-MM-dd HH:mm:ss")
	@JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss", timezone = "GMT+8")
	@Column(columnDefinition = "TIMESTAMP NULL DEFAULT NULL ON UPDATE CURRENT_TIMESTAMP COMMENT '更改时间'")
	public Date mtime;

	@ManyToOne(cascade = { CascadeType.PERSIST, CascadeType.REMOVE })
	@JoinColumn(name = "pid", referencedColumnName = "id")
	private Category categorys;

	@JsonIgnore
	@OneToMany(cascade = CascadeType.ALL, mappedBy = "category", fetch = FetchType.EAGER)
	private Set<Category> category;

	public int getId() {
		return id;
	}

	public void setId(int id) {
		this.id = id;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public String getDescription() {
		return description;
	}

	public void setDescription(String description) {
		this.description = description;
	}

	public String getPath() {
		return path;
	}

	public void setPath(String path) {
		this.path = path;
	}

	public String getStatus() {
		return status;
	}

	public void setStatus(String status) {
		this.status = status;
	}

	public Date getCtime() {
		return ctime;
	}

	public void setCtime(Date ctime) {
		this.ctime = ctime;
	}

	public Date getMtime() {
		return mtime;
	}

	public void setMtime(Date mtime) {
		this.mtime = mtime;
	}

	public Category getCategorys() {
		return categorys;
	}

	public void setCategorys(Category categorys) {
		this.categorys = categorys;
	}

	public Set<Category> getCategory() {
		return category;
	}

	public void setCategory(Set<Category> category) {
		this.category = category;
	}

	@Override
	public String toString() {
		return "Category [id=" + id + ", name=" + name + ", description=" + description + ", path=" + path + ", status="
				+ status + ", ctime=" + ctime + ", mtime=" + mtime + ", categorys=" + categorys + ", category="
				+ category + "]";
	}

}

```

期望结果

```

CREATE TABLE `category` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `ctime` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '',
  `description` varchar(255) DEFAULT NULL,
  `mtime` timestamp NULL DEFAULT NULL ON UPDATE CURRENT_TIMESTAMP COMMENT '',
  `name` varchar(255) DEFAULT NULL,
  `path` varchar(255) DEFAULT NULL,
  `status` enum('Enabled','Disabled') DEFAULT 'Enabled' COMMENT '',
  `pid` int(11) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8

```

### 为字段增加索引

我们希望为 name 和 path 字段增加普通索引

```

package cn.netkiller.domain;

import java.util.Date;
import java.util.Set;

import javax.persistence.CascadeType;
import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.FetchType;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.Index;
import javax.persistence.JoinColumn;
import javax.persistence.ManyToOne;
import javax.persistence.OneToMany;
import javax.persistence.Table;

import org.springframework.format.annotation.DateTimeFormat;

import com.fasterxml.jackson.annotation.JsonFormat;
import com.fasterxml.jackson.annotation.JsonIgnore;

@Entity
@Table(indexes = { @Index(name = "name", columnList = "name DESC"), @Index(name = "path", columnList = "path") })
public class Category {
	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	@Column(name = "id", unique = true, nullable = false, insertable = true, updatable = false)
	public int id;
	public String name;
	public String description;
	public String path;

	@Column(columnDefinition = "enum('Enabled','Disabled') DEFAULT 'Enabled' COMMENT '状态'")
	public String status;

	@DateTimeFormat(pattern = "yyyy-MM-dd HH:mm:ss")
	@JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss", timezone = "GMT+8")
	@Column(columnDefinition = "TIMESTAMP DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间'")
	public Date ctime;

	@DateTimeFormat(pattern = "yyyy-MM-dd HH:mm:ss")
	@JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss", timezone = "GMT+8")
	@Column(columnDefinition = "TIMESTAMP NULL DEFAULT NULL ON UPDATE CURRENT_TIMESTAMP COMMENT '更改时间'")
	public Date mtime;

	@ManyToOne(cascade = { CascadeType.PERSIST, CascadeType.REMOVE })
	@JoinColumn(name = "pid", referencedColumnName = "id")
	private Category categorys;

	@JsonIgnore
	@OneToMany(cascade = CascadeType.ALL, mappedBy = "category", fetch = FetchType.EAGER)
	private Set<Category> category;

	public int getId() {
		return id;
	}

	public void setId(int id) {
		this.id = id;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public String getDescription() {
		return description;
	}

	public void setDescription(String description) {
		this.description = description;
	}

	public String getPath() {
		return path;
	}

	public void setPath(String path) {
		this.path = path;
	}

	public String getStatus() {
		return status;
	}

	public void setStatus(String status) {
		this.status = status;
	}

	public Date getCtime() {
		return ctime;
	}

	public void setCtime(Date ctime) {
		this.ctime = ctime;
	}

	public Date getMtime() {
		return mtime;
	}

	public void setMtime(Date mtime) {
		this.mtime = mtime;
	}

	public Category getCategorys() {
		return categorys;
	}

	public void setCategorys(Category categorys) {
		this.categorys = categorys;
	}

	public Set<Category> getCategory() {
		return category;
	}

	public void setCategory(Set<Category> category) {
		this.category = category;
	}

	@Override
	public String toString() {
		return "Category [id=" + id + ", name=" + name + ", description=" + description + ", path=" + path + ", status="
				+ status + ", ctime=" + ctime + ", mtime=" + mtime + ", categorys=" + categorys + ", category="
				+ category + "]";
	}

}

```

期望结果

```

CREATE TABLE `category` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `ctime` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '????',
  `description` varchar(255) DEFAULT NULL,
  `mtime` timestamp NULL DEFAULT NULL ON UPDATE CURRENT_TIMESTAMP COMMENT '????',
  `name` varchar(255) DEFAULT NULL,
  `path` varchar(255) DEFAULT NULL,
  `status` enum('Enabled','Disabled') DEFAULT 'Enabled' COMMENT '??',
  `pid` int(11) DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `name` (`name`),
  KEY `path` (`path`),
  KEY `FKeiel7nqjxu4kmefso9tm9qcsu` (`pid`),
  CONSTRAINT `FKeiel7nqjxu4kmefso9tm9qcsu` FOREIGN KEY (`pid`) REFERENCES `category` (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8		

```

### 复合索引

创建由多个字段组成的复合索引，如： "member_id", "articleId"

```

package cn.netkiller.api.model;

import java.io.Serializable;
import java.util.Date;

import javax.persistence.CascadeType;
import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.JoinColumn;
import javax.persistence.ManyToOne;
import javax.persistence.Table;
import javax.persistence.Temporal;
import javax.persistence.TemporalType;
import javax.persistence.UniqueConstraint;

import com.fasterxml.jackson.annotation.JsonFormat;

@Entity
@Table(name = "comment", uniqueConstraints = { @UniqueConstraint(columnNames = { "member_id", "articleId" }) })
public class Comment implements Serializable {
	/**
	 * 
	 */
	private static final long serialVersionUID = -1484408775034277681L;
	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	@Column(name = "id", unique = true, nullable = false, insertable = true, updatable = false)
	private int id;

	@ManyToOne(cascade = { CascadeType.ALL })
	@JoinColumn(name = "member_id")
	private Member member;

	private int articleId;

	private String message;

	@JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss")
	@Temporal(TemporalType.TIMESTAMP)
	@Column(updatable = false)
	@org.hibernate.annotations.CreationTimestamp
	protected Date createDate;

	public int getId() {
		return id;
	}

	public void setId(int id) {
		this.id = id;
	}

	public Member getMember() {
		return member;
	}

	public void setMember(Member member) {
		this.member = member;
	}

	public int getArticleId() {
		return articleId;
	}

	public void setArticleId(int articleId) {
		this.articleId = articleId;
	}

	public String getMessage() {
		return message;
	}

	public void setMessage(String message) {
		this.message = message;
	}

	public Date getCreateDate() {
		return createDate;
	}

	public void setCreateDate(Date createDate) {
		this.createDate = createDate;
	}
}

```

期望结果

```

CREATE TABLE `comment` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `article_id` int(11) NOT NULL,
  `create_date` datetime DEFAULT NULL,
  `message` varchar(255) DEFAULT NULL,
  `member_id` int(11) DEFAULT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `UK5qxfiu92nwlvgli7bl3evl11m` (`member_id`,`article_id`),
  CONSTRAINT `FKmrrrpi513ssu63i2783jyiv9m` FOREIGN KEY (`member_id`) REFERENCES `member` (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

```

### 一对多实例

如下图，我们将实现 categroy 和 article 的一对多关系

```

      +-----------+
      | category  |
      |-----------|
  +-->|id         | <---+
  |   |title      |     |
  |   |description|    1:n
  |   |status     |     |
  |   |parent_id  | o---+
  |   +-----------+
  |
 1:n
  |
  |   +-----------------+            +-----------------+
  |   | article         |            | feedback        |
  |   |-----------------|            |-----------------|
  |   |id               |<--1:n--+   |id               |
  |   |title            |        |   |title            |
  |   |content          |        |   |content          |
  |   |datetime         |        |   |datetime         |
  |   |status           |        |   |status           |
  +--o|category_id      |        +--o|article_id       |
  +--o|member_id        |        +-->|member_id        |
  |   +-----------------+        |   +-----------------+
  |   | 2007,2008,2009  |        |   | 2007,2008,2009  |
  |   +-----------------+        |   +-----------------+
  |                              |
 1:n  +----------+     +---1:n---+
  |   | member   |     |
  |   |----------|     |
  +-->|id        | <---+
      |user      |
      |passwd    |
      |nickname  |
      |status    |
      +----------+

```

首先定义分类实体类

```

package cn.netkiller.domain;

import java.util.Date;
import java.util.Set;

import javax.persistence.CascadeType;
import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.FetchType;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.Index;
import javax.persistence.JoinColumn;
import javax.persistence.ManyToOne;
import javax.persistence.OneToMany;
import javax.persistence.Table;

import org.springframework.format.annotation.DateTimeFormat;

import com.fasterxml.jackson.annotation.JsonFormat;
import com.fasterxml.jackson.annotation.JsonIgnore;

@Entity
@Table(indexes = { @Index(name = "name", columnList = "name DESC"), @Index(name = "path", columnList = "path") })
public class Category {
	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	@Column(name = "id", unique = true, nullable = false, insertable = true, updatable = false)
	public int id;
	public String name;
	public String description;
	public String path;

	@Column(columnDefinition = "enum('Enabled','Disabled') DEFAULT 'Enabled' COMMENT '状态'")
	public String status;

	@DateTimeFormat(pattern = "yyyy-MM-dd HH:mm:ss")
	@JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss", timezone = "GMT+8")
	@Column(columnDefinition = "TIMESTAMP DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间'")
	public Date ctime;

	@DateTimeFormat(pattern = "yyyy-MM-dd HH:mm:ss")
	@JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss", timezone = "GMT+8")
	@Column(columnDefinition = "TIMESTAMP NULL DEFAULT NULL ON UPDATE CURRENT_TIMESTAMP COMMENT '更改时间'")
	public Date mtime;

	@ManyToOne(cascade = { CascadeType.PERSIST, CascadeType.REMOVE })
	@JoinColumn(name = "pid", referencedColumnName = "id")
	private Category categorys;

	@JsonIgnore
	@OneToMany(cascade = CascadeType.ALL, mappedBy = "category", fetch = FetchType.EAGER)
	private Set<Category> category;

	@JsonIgnore
	@OneToMany(cascade = CascadeType.ALL, mappedBy = "category", orphanRemoval = true)
	private Set<Article> article;

	public int getId() {
		return id;
	}

	public void setId(int id) {
		this.id = id;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public String getDescription() {
		return description;
	}

	public void setDescription(String description) {
		this.description = description;
	}

	public String getPath() {
		return path;
	}

	public void setPath(String path) {
		this.path = path;
	}

	public String getStatus() {
		return status;
	}

	public void setStatus(String status) {
		this.status = status;
	}

	public Date getCtime() {
		return ctime;
	}

	public void setCtime(Date ctime) {
		this.ctime = ctime;
	}

	public Date getMtime() {
		return mtime;
	}

	public void setMtime(Date mtime) {
		this.mtime = mtime;
	}

	public Category getCategorys() {
		return categorys;
	}

	public void setCategorys(Category categorys) {
		this.categorys = categorys;
	}

	public Set<Category> getCategory() {
		return category;
	}

	public void setCategory(Set<Category> category) {
		this.category = category;
	}

	@Override
	public String toString() {
		return "Category [id=" + id + ", name=" + name + ", description=" + description + ", path=" + path + ", status="
				+ status + ", ctime=" + ctime + ", mtime=" + mtime + ", categorys=" + categorys + ", category="
				+ category + "]";
	}

}

```

定义文章实体类

```

package cn.netkiller.domain;

import java.io.Serializable;
import java.util.Date;
import java.util.Set;

import javax.persistence.CascadeType;
import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.FetchType;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.JoinColumn;
import javax.persistence.ManyToOne;
import javax.persistence.OneToMany;
import javax.persistence.Table;

import org.springframework.format.annotation.DateTimeFormat;

import com.fasterxml.jackson.annotation.JsonFormat;
import com.fasterxml.jackson.annotation.JsonIgnore;

@Entity
@Table(name = "article")
public class Article implements Serializable {
	private static final long serialVersionUID = 7603772682950271321L;

	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	@Column(name = "id", unique = true, nullable = false, insertable = true, updatable = false)
	public int id;
	public String title;
	@Column(name = "short")
	public String shortTitle;
	public String description;
	public String author;
	public int star;
	public String tag;
	public boolean share;
	public boolean status;
	public String content;

	@ManyToOne(cascade = { CascadeType.PERSIST, CascadeType.REMOVE })
	@JoinColumn(name = "category_id", referencedColumnName = "id")
	private Category category;

	@ManyToOne(cascade = { CascadeType.PERSIST, CascadeType.REMOVE })
	@JoinColumn(name = "site_id", referencedColumnName = "id")
	private Site site;

	@ManyToOne(cascade = { CascadeType.ALL })
	@JoinColumn(name = "member_id", referencedColumnName = "id")
	private Member member;

	@DateTimeFormat(pattern = "yyyy-MM-dd HH:mm:ss")
	@JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss", timezone = "GMT+8")
	@Column(columnDefinition = "TIMESTAMP DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间'")
	public Date ctime;

	@DateTimeFormat(pattern = "yyyy-MM-dd HH:mm:ss")
	@JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss", timezone = "GMT+8")
	@Column(columnDefinition = "TIMESTAMP NULL DEFAULT NULL ON UPDATE CURRENT_TIMESTAMP COMMENT '更改时间'")
	public Date mtime;

	@JsonIgnore
	@OneToMany(cascade = CascadeType.ALL, mappedBy = "article", fetch = FetchType.EAGER)
	private Set<Comment> comment;

	@JsonIgnore
	@OneToMany(cascade = CascadeType.ALL, mappedBy = "article", fetch = FetchType.EAGER)
	private Set<Favorites> favorites;

	@JsonIgnore
	@OneToMany(cascade = CascadeType.ALL, mappedBy = "article", orphanRemoval = true)
	private Set<Statistics> statistics;

	public int getId() {
		return id;
	}

	public void setId(int id) {
		this.id = id;
	}

	public String getTitle() {
		return title;
	}

	public void setTitle(String title) {
		this.title = title;
	}

	public String getDescription() {
		return description;
	}

	public void setDescription(String description) {
		this.description = description;
	}

	public Date getCtime() {
		return ctime;
	}

	public void setCtime(Date ctime) {
		this.ctime = ctime;
	}

	public String getShortTitle() {
		return shortTitle;
	}

	public void setShortTitle(String shortTitle) {
		this.shortTitle = shortTitle;
	}

	public String getAuthor() {
		return author;
	}

	public void setAuthor(String author) {
		this.author = author;
	}

	public int getStar() {
		return star;
	}

	public void setStar(int star) {
		this.star = star;
	}

	public String getTag() {
		return tag;
	}

	public void setTag(String tag) {
		this.tag = tag;
	}

	public boolean isShare() {
		return share;
	}

	public void setShare(boolean share) {
		this.share = share;
	}

	public boolean isStatus() {
		return status;
	}

	public void setStatus(boolean status) {
		this.status = status;
	}

	public String getContent() {
		return content;
	}

	public void setContent(String content) {
		this.content = content;
	}

	public Category getCategory() {
		return category;
	}

	public void setCategory(Category category) {
		this.category = category;
	}

	public Member getMember() {
		return member;
	}

	public void setMember(Member member) {
		this.member = member;
	}

	public Set<Comment> getComment() {
		return comment;
	}

	public void setComment(Set<Comment> comment) {
		this.comment = comment;
	}

	public Set<Favorites> getFavorites() {
		return favorites;
	}

	public void setFavorites(Set<Favorites> favorites) {
		this.favorites = favorites;
	}

	public Set<Statistics> getStatistics() {
		return statistics;
	}

	public void setStatistics(Set<Statistics> statistics) {
		this.statistics = statistics;
	}

	public Site getSite() {
		return site;
	}

	public void setSite(Site site) {
		this.site = site;
	}

	public Date getMtime() {
		return mtime;
	}

	public void setMtime(Date mtime) {
		this.mtime = mtime;
	}

	@Override
	public String toString() {
		return "Article [id=" + id + ", title=" + title + ", shortTitle=" + shortTitle + ", description=" + description
				+ ", author=" + author + ", star=" + star + ", tag=" + tag + ", share=" + share + ", status=" + status
				+ ", content=" + content + ", category=" + category + ", site=" + site + ", member=" + member
				+ ", ctime=" + ctime + ", mtime=" + mtime + ", comment=" + comment + ", favorites=" + favorites
				+ ", statistics=" + statistics + "]";
	}

}

```

希望结果

```

CREATE TABLE `category` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `ctime` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '????',
  `description` varchar(255) DEFAULT NULL,
  `mtime` timestamp NULL DEFAULT NULL ON UPDATE CURRENT_TIMESTAMP COMMENT '????',
  `name` varchar(255) DEFAULT NULL,
  `path` varchar(255) DEFAULT NULL,
  `status` enum('Enabled','Disabled') DEFAULT 'Enabled' COMMENT '??',
  `pid` int(11) DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `name` (`name`),
  KEY `path` (`path`),
  KEY `FKeiel7nqjxu4kmefso9tm9qcsu` (`pid`),
  CONSTRAINT `FKeiel7nqjxu4kmefso9tm9qcsu` FOREIGN KEY (`pid`) REFERENCES `category` (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

CREATE TABLE `article` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `author` varchar(255) DEFAULT NULL,
  `content` varchar(255) DEFAULT NULL,
  `ctime` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '????',
  `description` varchar(255) DEFAULT NULL,
  `mtime` timestamp NULL DEFAULT NULL ON UPDATE CURRENT_TIMESTAMP COMMENT '????',
  `share` bit(1) NOT NULL,
  `short` varchar(255) DEFAULT NULL,
  `star` int(11) NOT NULL,
  `status` bit(1) NOT NULL,
  `tag` varchar(255) DEFAULT NULL,
  `title` varchar(255) DEFAULT NULL,
  `category_id` int(11) DEFAULT NULL,
  `member_id` int(11) DEFAULT NULL,
  `site_id` int(11) DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `FKy5kkohbk00g0w88fi05k2hcw` (`category_id`),
  KEY `FK6l9vkfd5ixw8o8kph5rj1k7gu` (`member_id`),
  KEY `FKrxbc33rok9m4n6pnbbwb3piwf` (`site_id`),
  CONSTRAINT `FK6l9vkfd5ixw8o8kph5rj1k7gu` FOREIGN KEY (`member_id`) REFERENCES `member` (`id`),
  CONSTRAINT `FKrxbc33rok9m4n6pnbbwb3piwf` FOREIGN KEY (`site_id`) REFERENCES `site` (`id`),
  CONSTRAINT `FKy5kkohbk00g0w88fi05k2hcw` FOREIGN KEY (`category_id`) REFERENCES `category` (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

```

现在我们已经将 categroy 与 article 两张表一对多关系建立起来。

### ManyToMany 多对多

用户与角色就是一个多对多的关系，多对多是需要中间表做关联的。所以需要一个 user_has_role 表。

```

    +----------+          +---------------+            +--------+
    | users    |          | user_has_role |            | role   |
    +----------+          +---------------+            +--------+
    | id       | <------o | user_id       |      /---> | id     |
    | name     |          | role_id       | o---+      | name   |
    | password |          |               |            |        |
    +----------+          +---------------+            +--------+

```

创建 User 表

```

package cn.netkiller.api.domain.test;

import java.io.Serializable;
import java.util.Set;

import javax.persistence.Entity;
import javax.persistence.FetchType;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.JoinTable;
import javax.persistence.ManyToMany;
import javax.persistence.Table;
import javax.persistence.JoinColumn;

@Entity
@Table(name = "users")
public class Users implements Serializable {
	/**
	 * 
	 */
	private static final long serialVersionUID = -2480194112597046349L;
	@Id
	@GeneratedValue(strategy = GenerationType.AUTO)
	private int id;
	private String name;
	private String password;

	@ManyToMany(fetch = FetchType.EAGER)
	@JoinTable(name = "user_has_role", joinColumns = { @JoinColumn(name = "user_id", referencedColumnName = "id") }, inverseJoinColumns = { @JoinColumn(name = "role_id", referencedColumnName = "id") })
	private Set<Roles> roles;

	public int getId() {
		return id;
	}

	public void setId(int id) {
		this.id = id;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public String getPassword() {
		return password;
	}

	public void setPassword(String password) {
		this.password = password;
	}

	public Set<Roles> getRoles() {
		return roles;
	}

	public void setRoles(Set<Roles> roles) {
		this.roles = roles;
	}

	@Override
	public String toString() {
		return "Users [id=" + id + ", name=" + name + ", password=" + password + ", roles=" + roles + "]";
	}

}

```

创建 Role 表

```

package cn.netkiller.api.domain.test;

import java.io.Serializable;
import java.util.Set;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.ManyToMany;
import javax.persistence.Table;

@Entity
@Table(name = "roles")
public class Roles implements Serializable {
	private static final long serialVersionUID = 6737037465677800326L;
	@Id
	@GeneratedValue(strategy = GenerationType.AUTO)
	private int id;
	private String name;
	@ManyToMany(mappedBy = "roles")
	private Set<Users> users;

	public int getId() {
		return id;
	}

	public void setId(int id) {
		this.id = id;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public Set<Users> getUsers() {
		return users;
	}

	public void setUsers(Set<Users> users) {
		this.users = users;
	}

	@Override
	public String toString() {
		return "Roles [id=" + id + ", name=" + name + ", users=" + users + "]";
	}

}

```

最终产生数据库表如下

```

CREATE TABLE `users` (
	`id` INT(11) NOT NULL AUTO_INCREMENT,
	`name` VARCHAR(255) NULL DEFAULT NULL,
	`password` VARCHAR(255) NULL DEFAULT NULL,
	PRIMARY KEY (`id`)
)
COLLATE='utf8_general_ci'
ENGINE=InnoDB;	

CREATE TABLE `roles` (
	`id` INT(11) NOT NULL AUTO_INCREMENT,
	`name` VARCHAR(255) NULL DEFAULT NULL,
	PRIMARY KEY (`id`)
)
COLLATE='utf8_general_ci'
ENGINE=InnoDB;

CREATE TABLE `user_has_role` (
	`user_id` INT(11) NOT NULL,
	`role_id` INT(11) NOT NULL,
	PRIMARY KEY (`user_id`, `role_id`),
	INDEX `FKsvvq61v3koh04fycopbjx72hj` (`role_id`),
	CONSTRAINT `FK2dl1ftxlkldulcp934i3125qo` FOREIGN KEY (`user_id`) REFERENCES `users` (`id`),
	CONSTRAINT `FKsvvq61v3koh04fycopbjx72hj` FOREIGN KEY (`role_id`) REFERENCES `roles` (`id`)
)
COLLATE='utf8_general_ci'
ENGINE=InnoDB;

```

### 外键级联删除

orphanRemoval = true 可以实现数据级联删除

```

package cn.netkiller.api.domain;

import java.io.Serializable;
import java.util.Set;

import javax.persistence.CascadeType;
import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.OneToMany;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Table;

import com.fasterxml.jackson.annotation.JsonIgnore;

@Entity
@Table(name = "member")
public class Member implements Serializable {
	/**
	 * 
	 */
	private static final long serialVersionUID = 1L;

	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	@Column(name = "id", unique = true, nullable = false, insertable = true, updatable = false)
	private int id;

	private String name;
	private String sex;
	private int age;
	private String wechat;

	@Column(unique = true)
	private String mobile;
	private String picture;
	private String ipAddress;

	@JsonIgnore
	@OneToMany(cascade = CascadeType.ALL, orphanRemoval = true, mappedBy = "member")
	private Set<Comment> comment;
	@JsonIgnore
	@OneToMany(cascade = CascadeType.ALL, orphanRemoval = true, mappedBy = "member")
	private Set<StatisticsHistory> statisticsHistory;

	public Member() {
	}

	public Member(int id) {
		this.id = id;
	}

	public int getId() {
		return id;
	}

	public void setId(int id) {
		this.id = id;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public String getSex() {
		return sex;
	}

	public void setSex(String sex) {
		this.sex = sex;
	}

	public int getAge() {
		return age;
	}

	public void setAge(int age) {
		this.age = age;
	}

	public String getWechat() {
		return wechat;
	}

	public void setWechat(String wechat) {
		this.wechat = wechat;
	}

	public String getMobile() {
		return mobile;
	}

	public void setMobile(String mobile) {
		this.mobile = mobile;
	}

	public String getPicture() {
		return picture;
	}

	public void setPicture(String picture) {
		this.picture = picture;
	}

	public String getIpAddress() {
		return ipAddress;
	}

	public void setIpAddress(String ipAddress) {
		this.ipAddress = ipAddress;
	}

	@Override
	public String toString() {
		return "Member [id=" + id + ", name=" + name + ", sex=" + sex + ", age=" + age + ", wechat=" + wechat + ", mobile=" + mobile + ", picture=" + picture + ", ipAddress=" + ipAddress + "]";
	}

}

```

## MongoDB

### 枚举定义

```

package api.pojo;

public class Enum {

	public enum Authority {
		USER, EXPERTS, ORGANIZATION, ADMIN
	}

	public enum Status {
		Enabled, Disabled, Yes, No, Y, N
	}

	public enum Examine {
		New, Pending, Rejected, Approved
	}

	public enum Task {
		New, Processing, Canceled, Completed
	}

	public enum Sex {
		True, False, T, F, Male, Female, Boy, Girl
	}

	public Enum() {
		// TODO Auto-generated constructor stub
	}
}

```

### 日志表

```

package api.domain;

import java.util.Date;

import org.springframework.data.annotation.Id;
import org.springframework.data.mongodb.core.mapping.Document;

@Document
public class Logging {

	public enum Priority {
		info, warning, error, critical, exception, debug
	}

	public enum Facility {
		app, sms, ios, android, api, db
	}

	@Id
	private String id;
	private String tag;
	private Date time;
	private Facility facility;
	private Priority priority;
	private String message;

	public Logging() {
		// TODO Auto-generated constructor stub
	}

	public String getId() {
		return id;
	}

	public void setId(String id) {
		this.id = id;
	}

	public String getTag() {
		return tag;
	}

	public void setTag(String tag) {
		this.tag = tag;
	}

	public Date getTime() {
		return time;
	}

	public void setTime(Date time) {
		this.time = time;
	}

	public Facility getFacility() {
		return facility;
	}

	public void setFacility(Facility facility) {
		this.facility = facility;
	}

	public Priority getPriority() {
		return priority;
	}

	public void setPriority(Priority priority) {
		this.priority = priority;
	}

	public String getMessage() {
		return message;
	}

	public void setMessage(String message) {
		this.message = message;
	}

	@Override
	public String toString() {
		return "Logging [id=" + id + ", tag=" + tag + ", time=" + time + ", facility=" + facility + ", priority=" + priority + ", message=" + message + "]";
	}

}

```

### 地址与定位

```

package cn.netkiller.api.domain;

import org.springframework.data.geo.Point;
import org.springframework.data.redis.core.index.GeoIndexed;
import org.springframework.data.redis.core.index.Indexed;

public class Address {

	private @Indexed String city;
	private String country;
	private @GeoIndexed Point location;

	public Address(String city, String country, Point location) {
		this.city = city;
		this.country = country;
		this.location = location;
	}

	public String getCity() {
		return city;
	}

	public void setCity(String city) {
		this.city = city;
	}

	public String getCountry() {
		return country;
	}

	public void setCountry(String country) {
		this.country = country;
	}

	public Point getLocation() {
		return location;
	}

	public void setLocation(Point location) {
		this.location = location;
	}

}			

```