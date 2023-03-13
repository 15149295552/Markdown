[toc]

# 数据库理论与应用

## 一、基本概念

### 1. 名词解释：数据库、数据库管理系统

- 数据库：根据某种数据模型进行组织，并存放到计算机存储设备的数据集合。笼统来讲，就是存放数据的仓库。
- 数据库管理系统：英文写作Database Management System（简称DBMS），是位于操作系统和用户之间的专门进行数据管理的软件系统。常见的DBMS： Oralce, MySQL,  DB2,  SQL Server,  Informix。

### 2. 请描述数据管理的三个阶段及每阶段特点

1）人工管理阶段：计算机不通过数据管理；程序和数据是不可分割的整体；数据不能共享；不单独保存数据；

2）文件系统阶段：数据以文件形式持久保存在外部存储设备上

- 优点：程序和数据实现了分离；数据的逻辑结构和物理结构有了区别；文件的建立，数据增、删、改、查都要用程序来实现；
- 缺点：数据冗余；不一致性；数据联系弱。

3）数据库管理阶段：单独使用一套软件系统来存储、管理数据

- 优点：
  数据独立性、可共享、低冗余
  数据库系统提供了方便、友好的接口（用户接口、程序接口）
  更强的安全性、可靠性
  丰富的工具（如性能优化、备份/恢复、查询、权限管理）
- 缺点：需付出额外的软件、硬件、人力成本

### 3. 名词解释：关系、关系数据库

- 关系：英文写作Relationship，由行、列组成的规范的二维表（每个属性不能重名、不可再分，关系中每一行次序不重要），行描述一个实体（事物）信息，列表示实体的属性。
- 关系数据库：是指使用关系（二维表）实体和实体间的联系的数据库管理系统。

### 4. 名词解释

- 实体（Entry）：现实中可以区分的事物称为实体
- 元组（Tuple）：表中的一行（也叫一条记录），表示一个实体
- 属性（Attribute）：表中的一列，描述实体的一个数据值
- 键（Key）：关系中唯一区分不同元组的属性或属性组合
- 主键（Primary Key）：多个键中选取一个作为主键，从逻辑上唯一确定一个实体的依据

### 5. 什么是主键，主键有什么要求

主键（Primary Key）指从多个键中选取一个作为主键，从逻辑上唯一确定一个实体的依据，主键的要求是非空、唯一。

### 6. 什么是笛卡尔积

两个集合的乘积，表示用集合中的元素两两组合，产生的新集合

### 7. 什么是数据库约束，列举常见的约束

1）约束（constraint），是用来保证数据完整性、一致性、有效性的规则，可以限制无效的数据进入数据库中，从数据库层面上提供了"安检"。

2）常见约束

- 非空约束：字段的值不能为空
- 唯一约束：字段的值必须唯一
- 主键约束：字段作为主键，非空、唯一
- 默认约束：未填写值的情况下，自动填写默认值
- 自动增加：字段值自动增加
- 外键约束

### 8. 什么是数据库索引

索引是一种提高查询的技术，通过对数据表中一列或多列数据进行排序，在查询时避免全表扫描，从而提高查询效率。索引是一种单独存放的数据结构，包含着数据表中所有记录的引用指针，根据该指针能快速找到数据所存储的物理位置。

### 9. 什么是普通索引、唯一索引

- 普通索引：MySQL中索引的基本类型，允许在定义索引的列中插入空值、重复值
- 唯一索引：索引的值必须唯一，允许有空值

### 10. 什么是聚簇索引、非聚簇索引

- 聚簇索引：数据和索引（B+树）在一起，记录被真实地保存在索引的叶子中，索引键值的逻辑顺序与索引所服务的表中相应行的物理顺序相同
- 非聚簇索引：数据和索引分开存储

### 11. 说明适合及不适合使用索引的情况

1）适合使用索引的情况

- 在经常查询、排序、分组的列使用索引
- 数据分布相对均匀、连续的字段，适合使用索引
- 查询操作较多的表上，适合创建索引

2）不适合使用索引的情况

- 如果表的数据量很少，不建议使用索引
- 经常更新的表不宜使用过多索引
- 避免在取值范围很少的列上使用索引（例如性别，账户状态）
- 二进制字段不适合建索引

### 12.哪些SQL语句可能导致索引失效

- 未使用索引字段作条件
- 条件判断中使用了不等于(<>)，会导致放弃使用索引进行全表扫描

```sql
select * from acct where acct_no <> '0001'
```

- 条件判断中使用null值判断，会导致放弃使用索引进行全表扫描

```sql
select * from acct where acct_no is null
```

- 模糊查询中，%前置会导致放弃使用索引进行全表扫描

```sql
select * from acct where acct_name like '%y'
```

- 对字段做运算，会导致放弃使用索引进行全表扫描

```sql
select * from acct where balance + 100 > 1000
select * from acct where substr(acct_name, 1, 3) = "ABC"
```

### 13. 什么是事务，事务的用是什么

1）事务（Transaction）：指执行的一系列操作，要么全都执行，要么全都不执行

2）事务的作用：保证数据的正确性、一致性

### 14. 描述事物的ACID特性

- 原子性（Atomicity）：一个事务是不可分割的整体，要么全都执行，要么全都不执行
- 一致性（Consistency）：事务执行完成后，数据库从一个一致性状态变成另一个一致性状态
- 隔离性（Isolation）：不同的事务不相互影响、干扰
- 持久性（Durability）：一旦事务提交，对数据库的修改就必须永久保留下来

### 15. 事务对数据库性能有什么影响

事务会对数据进行过加锁，一个事务正在操作数据时候，另一个事务要操作相同的数据，则必须等待，等待上一个事务提交。所以，事务会降低数据库的增删改效率，换来的好处是保证了数据一致性。

### 16. 什么是视图，使用视图有什么好处

1）视图（View），是一个查看数据的窗口，本身不存放数据，它定义了一组查询规则，使用户可以查看到感兴趣的数据。

2）使用视图的好处

- 高效：复杂的连接查询，每次执行时效率比较低，可以考虑新建视图，每次从视图中获取，将会提高效率。
- 安全：有的数据是需要保密的，如果直接把表给出来进行操作会造成泄密，那么可以通过创建视图把相应视图的权限给出来即可保证数据的安全。
- 减少数据的重复存储。

3）如何创建视图

```sql
CREATE VIEW v_acct_name  as
select acct_no, acct_name from acct; 
```

### 17. 什么是锁，锁有哪些分类

1）锁是对数据的控制权，其作用是解决多个工作单元并发操作数据的问题

2）锁的分类

- 按粒度分：
  - 行级锁（细粒度）：锁定某一行，并发效率高，资源消耗多
  - 表级锁（粗粒度）：一次直接锁定整张表，并发效率低，资源消耗少
- 按类型
  - 读锁（共享锁）：select时候加锁，加锁之后可以读取，不能写
  - 写锁（排它锁）: insert/update/delete时候加锁，加锁之后不能读、写

### 18. 什么是短连接、长连接、连接池

1）短连接。短连接是指程序和数据库通信时需要建立连接，执行操作后，连接关闭。短连接简单来说就是每一次操作数据库，都要打开和关闭数据库连接，基本步骤是：连接→数据传输→关闭连接。

2）长连接是指程序之间的连接在建立之后，就一直打开，被后续程序重用。

3）事先建立一组连接，这一组连接被称为连接池。连接池将被所有连接的客户端共享使用，连接池可以加速连接，也可以减少数据库连接，降低数据库服务器的负载。

### 19. 什么是SQL语句执行计划

执行计划就是一系列的操作步骤。SQL是声明性语言，它只告诉数据库要查询什么，但并不告诉数据库如何去查。数据库所要做的就是基于算法和统计信息计算出一条最佳的访问路径。这个工作是由优化器来完成的。优化器会比较不同的执行计划，然后选择其中最优的一套。

### 20. 说明SQL语句执行过程

客户端（Clients）首先连接MySQL，然后发布查询，如果缓存（Query Cache）中有结果集，则直接返回结果集。如果结果没有被缓存，那么，MySQL解析查询（Parser）将通过优化器（Optimizer）生成执行计划，然后运行执行计划通过API（Pluggable Storage Engine API）从存储引擎获取数据，并返回给客户端。SQL语句执行时子句执行步骤如下所示：

- 第一步：首先执行from子句，从表中找到源数据
- 第二步：执行where子句，选出所有满足条件的数据
- 第三步：group by子句进行分组
- 第四步：聚合操作
- 第五步：having子句对聚合结果进行过滤
- 第六步：order by子句对结果进行排序
- 第七步：limit限制显示笔数

### 21. MySQL中in和exists的区别

1）in：进行多次比较（二分法）

2）exists：只判断记录是否存在，该子查询实际上并不返回任何数据，而是返回值True或False，exists指定一个子查询，检测行的存在

3）使用in还是exists：如果子查询的数据小于外查询，适合使用in；如果子查询的数据量大于外查询，适合使用exists

```mysql
-- 根据客户姓名查询该客户账号，从账户表查询账号在子查询结果中的记录
select * from acct 
where acct_no in (select acct_no from customer where acct_name='Jerry')

-- 从客户表查询指定账号的信息
select * from customer c
where EXISTS (select cust_no from acct a 
              where cust_no='C0001' 
              and a.cust_no = c.cust_no)
```



## 二、MySQL基本语句

### 1. 写出Linux下启动、停止、查看MySQL服务状态的命令

- 查看服务状态： /etc/init.d/mysql status
- 启动服务:  /etc/init.d/mysql start
- 停止服务 :  /etc/init.d/mysql stop

### 2. 什么是存储引擎，说明MySQL常见存储引擎特点及适用场景

1）定义。存储引擎是指表的存储方式，包括：存储机制、索引机制、锁定方式

2）常见存储引擎

① InnoDB

- 特性：支持事务、行级锁、外键；共享表空间
- 适用场合：更新密集的表；有支持事务的要求；自动灾备和恢复；有外键约束要求；支持自动增加列auto_increment属性

② MyISAM

- 特性：支持表级锁，不支持事务、外键、行锁定；访问速度快；独享表空间；数据库宕机后，MyISAM表容易损坏，灾难恢复性不佳
- 适用场合：查询请求较多；数据一致性要求较低；没有外键约束

③ Memory

- 特性：表结构存储于硬盘，表记录存于于内存；服务器重启后，表记录消失
- 适用场合：数据量小；数据丢失不会造成影响；访问速度要求高

### 3. MySQL中有哪些跟权限相关的表

- user表：最重要的权限表，记录允许连接到服务器的账号信息和权限
- db表：记录库的授权信息
- tables_priv表：记录表的授权信息
- columns_priv表：记录授权的字段信息

### 4. 按要求编写SQL语句

1）创建数据库bank, 并指定为utf8编码格式

```mysql
create database bank default charset=utf8;
```

2）编写SQL语句，创建表并插入数据

- 创建账户表(acct, utf8编码格式), 包含如下字段，并插入数据：

| 字段名称  | 中文名称 | 类型                                | 长度              |备注|
| --------- | -------- | ----------------------------------- | ----------------- |----|
| acct_no   | 账号     | 字符串类型                          | 32            ||
| acct_name | 户名     | 字符串类型                          | 128           ||
| cust_no   | 客户编号 | 字符串类型                          | 32                ||
| acct_type | 账户类型 | 整数型       |                   |1-存款账户 2-贷款账户|
| reg_date  | 开户日期 | 日期类型                            |                   ||
| status    | 账户状态 | 整数型 |                   |1-正常 2-注销 3-挂失 4-冻结|
| balance   | 账户余额 | 数字类型                            | 最长16位，2位小数 ||

建表：

```sql
create table acct(
	acct_no   varchar(32),     -- 字符串类型，长度32字节
	acct_name varchar(128),    -- 字符串类型，长度128字节
	cust_no   varchar(32),     -- 客户编号
	acct_type int,             -- 账户类型, 整数型(1-存款账户 2-贷款账户)
	reg_date  date,            -- 开户日期, 日期类型
	status    int,			   -- 账户状态
	balance   decimal(16,2)    -- 数字类型，最长16位，2位小数
) default charset = utf8;
```

插入数据：

```sql
insert into acct 
	values('622345000001', 'Jerry', 'C0001', 1, date(now()), 1, 5000.00);
insert into acct values
('622345000002', 'Tom', 'C0002', 1, date(now()), 1, 2000.00),
('622345000003', 'Dekie', 'C0003', 2, date(now()), 1, 3000.00),
('622345000004', 'Dokas', 'C0004', 2, date(now()), 2, 7000.00),
('622345000005', 'Steven', 'C0005', 2, date(now()), 3, 4000.00);
```



- 创建客户信息表(customer,utf8编码格式 )，包含字段如下所示，并为每个账户插入一笔对应的客户信息：

| 字段名称  | 中文名称 | 类型       | 长度 | 备注 |
| --------- | -------- | ---------- | ---- | ---- |
| cust_no   | 客户编号 | 字符串类型 | 32   | 主键 |
| tel_no    | 电话号码 | 字符串类型 | 32   | 非空 |
| cust_name | 客户姓名 | 字符串类型 | 64   | 非空 |
| address   | 地址     | 字符串类型 | 128  | 非空 |

建表：

```sql
create table customer (
    cust_no varchar(32) primary key,
    tel_no varchar(32) not null,
    cust_name varchar(64) not null,
    address varchar(128) not null
) charset = utf8;
```

插入数据：

```sql
insert into customer values
('C0001', '13512345678', 'Jerry', '北京'),
('C0002', '13222334455', 'Tom', '成都'),
('C0003', '13811112222', 'Dekie', '北京'),
('C0004', '15844556677', 'Dokas', '杭州'),
('C0005', '13255443333', 'Steven', '北京');
```



3）查找余额大于等于5000的贷款账户信息

```sql
select * from acct where balance >= 5000 and acct_type = 2;  
```

4）查找账户名称以'D'开头的所有账户信息

```sql
select * from acct where acct_name like 'D%'; 
```

5）统计每种状态的账户笔数

```sql
select status "账户状态", count(*) "笔数" from acct group by status;
```

6）查询账户余额的最大值、最小值、平均值、总金额

```sql
select max(balance), min(balance), avg(balance), sum(balance) from acct;
```

7）查询账户余额最大的前3个账户

```sql
select * from acct order by balance desc limit 3;
```

8）修改acct表结构

-   在acct_no上添加主键约束

  ```sql
  alter table acct add primary key(acct_no);
  ```

- 在acct_name, acct_type字段上添加非空约束

- ```sql
  alter table acct modify acct_name varchar(128) not null;
  alter table acct modify acct_type int not null;
  ```

- 在status字段添加默认约束，默认值为1

  ```mysql
  alter table acct modify status int default 1;
  ```

- 在reg_date字段上创建普通索引

  ```sql
  create index idx_reg_date on acct(reg_date);
  ```

9）查询所有贷款账户所属客户名称、电话号码（利用子查询实现）

```sql
select cust_name, tel_no from customer where cust_no in (
    select cust_no from acct where acct_type = 2
);
```

10）编写一个查询语句，查询结果包含如下字段：账号、户名、账户类型、余额、客户编号、客户电话、地址

```sql
select a.acct_no, a.acct_name, a.acct_type, a.balance,
       b.cust_no, b.tel_no, b.address
from acct a, customer b
where a.cust_no = b.cust_no;
```

11）以账户表作为左表，客户信息表作为右表，执行左连接查询，查询结果包括：账号、户名、账户类型、余额、客户编号、客户电话、地址

```mysql
select a.acct_no, a.acct_name, a.acct_type, a.balance,
       b.cust_no, b.tel_no, b.address
from acct a
left join customer b
on a.cust_no = b.cust_no;
```

12）新建账户交易明细表（acct_trans_detail，utf-8编码格式），包含的字段如下表，并按要求插入数据

| 字段名称   | 中文名称   | 类型     | 长度                  | 备注                                 |
| ---------- | ---------- | -------- | --------------------- | ------------------------------------ |
| trans_sn   | 交易流水号 | 字符串   | 32                    | 唯一索引                             |
| trans_date | 交易时间   | 日期时间 |                       | 非空                                 |
| acct_no    | 交易账号   | 字符串   | 32                    | 非空                                 |
| trans_type | 交易类型   | 整数型   |                       | 非空 （1-存款 2-取款 3-消费 4-结息） |
| amt        | 交易金额   | 浮点型   | 最长10位，小数部分2位 | 非空                                 |

- 建表

  ```mysql
  create table acct_trans_detail (
  	trans_sn 	varchar(32) not null,	-- 交易流水号
  	trans_date 	datetime not null, 		-- 交易日期
  	acct_no 	varchar(32) not null, 	-- 交易账号
  	trans_type 	int not null,    			-- 交易类型(存款、取款、刷卡、结息等等)
  	amt 		decimal(10,2) not null, -- 交易金额
  	unique(trans_sn),index(trans_date)  -- 在交易流水号上创建唯一索引, 交易日期上创建普通索引
  );
  ```

- 插入数据：为622345000001账户622345000002账户各插入一笔交易明细信息

  ```mysql
  insert into acct_trans_detail 
  	values('202001010001', now(), '622345000001', 1, 1000.00);
  insert into acct_trans_detail 
  	values('202001010002', now(), '622345000002', 1, 1000.00);
  ```

13）查询最近6个月没有发生过交易的账户所属客户名称、电话号码

```mysql
SELECT a.acct_no, b.cust_name, b.tel_no
  FROM acct a, customer b
 WHERE a.acct_no NOT IN (
     	-- 先查出最近留个月发生过交易的账户
		SELECT acct_no
		  FROM acct_trans_detail
		 WHERE trans_date > DATE_SUB(now(), INTERVAL 6 MONTH)
	)
AND a.cust_no = b.cust_no
```

### 5. 按要求编写SQL语句

1）创建订单表(orders)，包含字段如下所示，并插入数据

| 字段名称     | 中文名称 | 类型         | 长度 | 备注                                                         |
| ------------ | -------- | ------------ | ---- | ------------------------------------------------------------ |
| order_id     | 订单编号 | 字符串类型   | 32   | 主键                                                         |
| cust_id      | 客户编号 | 字符串类型   | 32   |                                                              |
| order_date   | 下单时间 | 日期时间类型 |      |                                                              |
| status       | 账户状态 | 枚举类型     |      | 1-待付款，2-待发货，3-已发货，4-已收货，5-申请退货，6-已退货，9-已废弃 |
| products_num | 商品数量 | 整数类型     |      |                                                              |
| amt          | 订单金额 | 浮点数       |      | 保留两位小数                                                 |

- 建表

  ```mysql
  create table orders (
      order_id varchar(32) primary key,
      cust_id varchar(32),
      order_date datetime,
      status enum('1','2','3','4','5','6','9'),
      products_num int,
      amt decimal(16,2)
  ) default charset=utf8;
  ```

- 插入数据

  ```mysql
  insert into orders values('201801010001', '0001', now(), '1', 2, 100);
  insert into orders values('201801010002', '0001', now(), '1', 1, 200);
  insert into orders values('201801020001', '0002', now(), '4', 1, 70);
  insert into orders values('201801020002', '0002', now(), '2', 3, 450.34);
  insert into orders values('201801020003', '0003', now(), '3', 4, 800.45);
  ```

2）查找所有已发货、已收货、申请退货的订单

```mysql
select * from orders where status in ('3','4','5');
```

3）查找某个客户待发货的订单

```mysql
select * from orders where cust_id = '0002' and status = '2';
```

4）根据订单编号，查找该订单的下单日期、订单状态信息

```mysql
select order_date, status from orders where order_id = '201801010001';
```
5）查找某个客户所有订单，并按照下单日期倒序排列

```mysql
select * from orders where cust_id = '0001' order by order_date desc;
```

6）在表的最后添加两个字段：
    invoice			发票开具状态, 整数型
    invoice_date	开票日期, DateTime类型

```mysql
alter table orders add invoice int;
alter table orders add invoice_date datetime;
```

7）修改某个订单的状态为已收货

```mysql
update orders set status = '4' where order_id = '201801010001';  
```

8）修改orders表结构

- 在cust_id, order_date, products_num字段上添加非空约束

  ```mysql
  alter table orders modify cust_id varchar(32) not null;
  alter table orders modify order_date datetime not null;
  alter table orders modify products_num int not null;
  ```

- 在status字段添加默认约束，默认值为1

  ```mysql
  alter table orders modify status enum('1','2','3','4','5','6','9') default '1';
  ```

- 在order_date字段上创建普通索引

  ```mysql
  create index idx_order_date on orders(order_date);
  ```

9）创建客户信息表customers，包含字段如下表所示，并插入数据

| 字段名称  | 中文名称 | 类型       | 长度 | 备注 |
| --------- | -------- | ---------- | ---- | ---- |
| cust_id   | 客户编号 | 字符串类型 | 32   | 主键 |
| cust_tel  | 电话号码 | 字符串类型 | 128  | 非空 |
| cust_name | 客户姓名 | 字符串类型 | 64   | 非空 |
| address   | 送货地址 | 字符串类型 | 128  | 非空 |

- 建表

  ```mysql
  create table customers (
      cust_id varchar(32) primary key,
      cust_tel varchar(32) not null,
      cust_name varchar(64) not null,
      address varchar(128) not null
  ) charset = utf8;
  ```

- 插入数据

  ```mysql
  insert into customers values
  ('0001', '13512345678', 'Jerry', '北京'),
  ('0002', '13222334455', 'Tom', '成都'),
  ('0003', '13811112222', 'Dekie', '北京');
  ```

10）在orders表的cust_id字段上创建外键约束，引用customers表的cust_id字段

```mysql
alter table orders 
add constraint fk_cust_id 
foreign key(cust_id) references customers(cust_id);
```

11）查询所有订单为状态为"申请退货"的客户名称、电话号码（利用子查询实现）

```mysql
select cust_name, cust_tel from customers where cust_id in (
    select distinct cust_id from orders where status = '5'
);
```

12）利用连接查询，查询"待送货"订单的订单信息、客户信息，查询结果包含的字段有：订单编号、下单时间、客户编号、客户姓名、客户电话、送货地址

```mysql
select o.order_id, o.order_date, o.cust_id, c.cust_name, c.cust_tel, c.address
from orders o, customers c
where o.status = '2'
 and o.cust_id = c.cust_id;
```

### 6. 写出高效SQL语句方法

- 在select,where,order by常涉及的列上建立索引

- 条件判断尽量避免使用不等于，否则放弃使用索引进行全表扫描

- 条件判断尽量避免使用null值判断，否则放弃使用索引进行全表扫描

- 条件判断尽量避免or连接

- 条件判断尽量避免使用in/not in

- 模糊查询尽量避免前置%

- 查询时尽量避免使用*, 而使用具体字段名称

- 尽量避免在where子句中，对字段进行计算。例如：
  
     ```mysql
     select * from acct where balance / 2 > 5000
     ```
     
- 数字类型字段，进行条件比较时比字符串效率更高（字符串要逐个字符进行比较）

- 联合查询中先对数据进行筛选再进行联合

- 拆分一些大的SQL，将其拆分为几个小的SQL组合

- WHERE条件语句中必须使用合适的类型，避免MySQL进行隐式类型转化

- INSERT语句必须显式地指明字段名称，不要使用INSERT INTO table()

- INSERT语句如果使用批量提交（如INSERT INTO table VALUES(),(),()……），那么VALUES的个数不应过多。一次性提交过多的记录，会导致线上I/O紧张

- 应尽量避免使用连接（JOIN），连接的表也不宜过多

## 三、企业真题

### 1. 选择题

1）使用SQL语句进行分组检索时，为了去掉不满足条件的分组，应当（B）
A. 使用WHERE子句
B. 在GROUP BY后面使用HAVING 子句
C. 先使用WHERE子句，再使用HAVING子句
D. 先使用HAVING 子句，再使用WHERE子句 



2）储蓄所有多个储户，储户在多个储蓄所存取款，储蓄所与储户之间是（D）

A．一对一的联系 

B．一对多的联系

C．多对一的联系 

D．多对多的联系 



3）视图是一个“虚表”，视图的构造基于（C）

A.基本表 

B.视图

C.基本表或视图 

D.数据字典



4）在SQL语句中，与表达式"仓库号 NOT IN（"wh1", "wh2"）"功能相同的表达式是（D ）

A）仓库号="wh1" AND 仓库号="wh2" 

B）仓库号!="wh1" OR 仓库号！= "wh2"

C）仓库号="wh1" OR 仓库号="wh2" 

D）仓库号!="wh1" AND 仓库号!="wh2" 



### 2. 按照要求编写SQL语句

现有如下表：

Bom_item(物料清单)

| id     | material_id(物料id) | material_name            | process_id(工序id) | process_name |
| ------ | ------------------- | ------------------------ | ------------------ | ------------ |
| 430764 | 710518              | NEW7-C00700A_四联单10A   | 8297               | 袋配（开关)  |
| 430768 | 679914              | 停销NEA3-1111_一联五孔   | 8298               | 装配(非插)   |
| 430777 | 675363              | 基座9A22610 8ZTE.024.777 | 8293               | 注望         |
| 430781 | 711113              | NEw7-S00100 8ZTE.064.580 | 8293               | 注塑         |

mod_resource(模具)

| id(模具id) | code               | process_id(工序id) | group_id | section_id |
| ---------- | ------------------ | ------------------ | -------- | ---------- |
| 72462      | 保护门NEW1-111-3M1 | 8293               | 1        | 7          |
| 72463      | 顶杆NEW1-0O2M6     | 8293               | 1        | 7          |
| 72464      | 盖板NEW1-C00100M1  | 8293               | 1        | 7          |
| 72465      | 盖板NEW1-CO0300M1  | 8293               | 1        | 7          |

product_mould_pair（物料指定模具表）

| group_id | match_id | material_id(物料id) | amount | resource_id(模具id) | xxx_id |
| -------- | -------- | ------------------- | ------ | ------------------- | ------ |
| 1        | 0        | 675363              | 1      | 72462               | 1      |
| 1        | 0        | 711113              | 1      | 72463               | 1      |
| 1        | 0        | 710518              | 1      | 72464               | 1      |
| 1        | 0        | 679914              | 1      | 72465               | 1      |

关联关系:
product_mould_pair.material_id = bom_item.material_id

product_mould_pair resource_id = mod_resource.id



1）在product_mould_pair表中需要添加一条关联数据, 物料id为675363,模具id为72463，编写sql语句

```mysql
insert into product_mould_pair(group_id, match_id, material_id, amount, resource_id, xxx_id) 
values(1, 0, 675363, 1, 72463, 1)
```



2）查出product_mould_pair表中materail_id和resource_id的process_id不一致的数据

```mysql
select pmp.material_id, pmp.resource_id, mr.process_id, bom.process_id
from product_mould_pair pmp, mod_resource mr, Bom_item bom 
where pmp.resource_id = mr.id
and bom.material_id = pmp.material_id
and mr.process_id <> bom.process_id
```



以下为测试使用的建表及插入语句

```mysql
create table bom_item(
	id   int,     
	material_id int,  
	material_name   varchar(32),   
	process_id int,      
	process_name  varchar(32)
) default charset = utf8;
insert into bom_item values 
(430764, 710518, 'NEW7-C00700A_四联单10A', 8297, '袋配（开关)'),
(430768, 679914, '停销NEA3-1111_一联五孔', 8298, '装配(非插)'),
(430777, 675363, '基座9A22610 8ZTE.024.777', 8293, '注望'),
(430781, 711113, 'NEw7-S00100 8ZTE.064.580', 8293, '注望');

create table mod_resource(
	id   int,    
	code varchar(32), 
	process_id int,      
	group_id int,  
	section_id int
) default charset = utf8;
insert into mod_resource values 
(72462, '保护门NEW1-111-3M1', 8293, 1, 7),
(72463, '顶杆NEW1-0O2M6', 8293, 1, 7),
(72464, '盖板NEW1-C00100M1', 8293, 1, 7),
(72465, '盖板NEW1-CO0300M1', 8293, 1, 7);

create table product_mould_pair(
	group_id   int,
  match_id int,    
	material_id int,      
	amount int,  
	resource_id int,
  xxx_id int
) default charset = utf8;
insert into product_mould_pair values 
(1, 0, 675363, 1, 72462, 1),
(1, 0, 711113, 1, 72463, 1),
(1, 0, 710518, 1, 72464, 1),
(1, 0, 679914, 1, 72465, 1);

```



### 3. 根据要求编写SQL语句

数据库中表字段如下：

| 运单号  | 客户id | 创建日期 |
| ------- | ------ | -------- |
| PNO0011 | CC001  | 2020/5/1 |

计算创建日期再0501-0530期间客户的单量分布情况，最终得出如下结果：

| 单量   | 客户数  |
| ------ | ------- |
| 0-5    | 1000000 |
| 6-10   | 200000  |
| 11-20  | 50000   |
| 20以上 | 30000   |

```mysql
select  case when `单量` <=5  then '0-5'
		 when `单量` between 6 and 10 then '6-10'
		 when `单量` between 11 and 20 then '11-20'
         else  '20以上'
end as '单量',
count(distinct `客户id`)  as '客户数' 
from (
    	-- 子查询先统计指定时间段每个客户订单数量
		select `客户id` , count(distinct `运单号`)  '单量' from t_orders
		where  `创建日期` between '2020/5/1' and  '2020/5/31'
		group by `客户id`
) as  t_sub
group by 1 -- 根据第1个字段排序
```



### 4. 根据要求编写SQL语句

用户每日登陆APP会访问不同模块，现有两个表，表1记录了用户的id以及在不同模块的活跃时长，表2记录了所有用户的一些属性。结构如下：

表1：dau

| d          | uid  | module  | active_duration |
| ---------- | ---- | ------- | --------------- |
| 2021-01-01 | 1    | jobs    | 324             |
| 2021-01-01 | 2    | feeds   | 445             |
| 2021-01-01 | 3    | im      | 345             |
| 2021-01-02 | 2    | network | 765             |
| 2021-01-02 | 3    | jobs    | 342             |
| ...        | ...  | ...     | ...             |

- d：活跃日期
- uid：用户唯一编码
- module：用户活跃模块
- active_duration：该模块下活跃时长（单位：s）



表2：users

| uid  | career_level | city   | work_length |
| ---- | ------------ | ------ | ----------- |
| 1    | 中级人才     | 北京   | 4           |
| 2    | 高级人才     | 上海   | 7           |
| 3    | 初级人才     | 北京   | 1           |
| 4    | 普通人才     | 平顶山 | 2           |
| ...  | ...          | ...    | ...         |

- uid：用户唯一编码
- career_level：人才级别
- city：城市
- work_length：工作年限



题目：

1）查询九月中，每日活跃的用户数

```mysql
select d, count(distinct uid) from dau
where month(d) = 9
group by d
```

2）查询九月中，对于活跃用户，其当日活跃时长最大的模块

```mysql
select d, module, max_u_m from 
(
    -- 从子查询解结果中查询日期，模块，统计每个模块最大用户数量
    -- over是窗口函数（窗口可以理解为将原数据划分成不同的范围，即分组）
	select *, max(u_count) over(partition by d, module) as mux_u_m from
	(
        -- 统计9月份每天、每个模块用户数量
		select d, module, count(distinct uid) u_count from dau
		where month(d) = 9 
		group by d, module
	) as t_sub1
)as t_sub2
where u_count = max_u_m
```

3）十一期间的活跃用户中，jobs模块活跃时间超过100s的用户

```mysql
select uid, sum(active_duration) as total_duration 
from dau
where (d >= '2020-10-01' and d <= '2020-10-07') and module = 'jobs'
group by uid
having total_duration > 100
```

4）过去一周内，北京用户的活跃总时长的均值

```mysql
select avg(total_duration) as dur_avg
from (
    select dau.uid, sum(active_duration) as total_duration
    from dau
    inner join users_info on d >= date_sub(current_date, interval 7 day)
                    and users_info.city = '北京'
                    and dau.uid = users_info.uid
    group by dau.uid    
) as t_sub;
```

5）如果想了解过去一个月中，不同人才级别的用户活跃频次差异，什么数据可以说明问题，请写出你的思考及原因，对应的SQL语句是什么？

答：对比不同用户级别浏览频次的平均值或中位数，当用户量多时受极值影响较小，可以查看均值或标准差；当用户量少时受极值影响较大，可以查看中位数

```mysql
select career_level, avg(login_count), stddev_samp(login_count) from 
(
    -- 子查询统计用户过去一个月登录次数
	select career_level, dau.uid, count(*) as login_count 
    from  dau 
	inner join users_info on d >= date_sub(current_date, interval 1 month)
              and dau.uid = users_info.uid
	group by career_level, dau.uid
) as t_sub
group by career_level
```



测试使用建表插入数据SQL（users表名容易冲突，实际使用users_info）：

```sql
create table dau (
	d varchar(16),
	uid int,
	module varchar(32),
	active_duration int
);
insert into dau VALUES
('2021-09-01', 1, 'jobs', 324),
('2021-09-01', 2, 'feeds', 445),
('2021-09-01', 3, 'im', 345),
('2021-09-02', 2, 'network', 765),
('2021-09-02', 3, 'jobs', 342),
('2021-10-01', 1, 'jobs', 324),
('2021-10-01', 2, 'feeds', 445),
('2021-10-01', 3, 'im', 345),
('2021-10-02', 2, 'network', 765),
('2021-10-02', 3, 'jobs', 342);

create table users_info (
	uid int,
	career_level varchar(32),
	city varchar(32),
	work_length int
);
insert into users_info VALUES
(1, '中级人才', '北京', 4),
(2, '高级人才', '上海', 7),
(3, '初级人才', '北京', 1),
(4, '普通人才', '平顶山', 2);
```



### 5. 按照要求编写SQL语句

1）求每日的客单价，表名afu_order，格式如下：

| userID | order_id | order_date | payment |
| ------ | -------- | ---------- | ------- |
| 1      | E1234    | 2021-04-01 | 299     |
| 1      | E1235    | 2021-04-01 | 199     |
| 3      | E1237    | 2021-04-01 | 39      |

```mysql
select order_date, sum(payment) / count(distinct userid) as '客单价'
from afu_order
group by order_date
```



2）求当日下单中新用户的订单数据

表1：订单历史表（不包含当日数据）   表名afu_order

| userID | order_id | order_date | payment |
| ------ | -------- | ---------- | ------- |
| 1      | E1234    | 2021-04-01 | 299     |
| 2      | E1237    | 2021-04-01 | 99      |
| 3      | E1248    | 2021-04-03 | 39      |
| 4      | E1250    | 2021-04-16 | 139     |
| ...    | ...      | ...        | ...     |

表2：当日订单表  表名afu_order_day

| userID | order_id | order_date | payment |
| ------ | -------- | ---------- | ------- |
| 1      | E1334    | 2021-04-18 | 299     |
| 2      | E1335    | 2021-04-18 | 99      |
| 3      | E1337    | 2021-04-18 | 39      |
| 4      | E1348    | 2021-04-18 | 139     |
| ...    | ...      | ...        | ...     |

```mysql
select count(distinct order_id) from afu_order_day
where user not in (
    select userID from afu_order group by userid
)
```



3）求每日销量TOP10的商品   表名afu_order

| order_id | sku_id   | product_name | payment | order_date |
| -------- | -------- | ------------ | ------- | ---------- |
| 111111   | A1TC3030 | 马迷         | 79      | 2021-01-12 |
| 111139   | A1TC3031 | 鸢尾         | 299     | 2021-01-12 |
| 111139   | A1TC3032 | 眼霜         | 29      | 2021-01-12 |

```mysql
select * from (
    -- 第二层子查询，使用窗口函数按日期、商品分组对销售金额进行排序
    -- row_number函数是mysql8.0引入的新函数，它从1开始为每一行分配一个序号
	select *, row_number over(partition by order_date, product_name order by total_sales desc) as rank from (
        -- 第一层子查询按日期、商品、商品id统计金额
        select order_date, product_name, sku_id, sum(payment) total_sales
        from afu_order 
        group by order_date, product_name, sku_id
    ) as t_sub1
) as t_sub2
where rank <= 10
```

