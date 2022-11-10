[toc]

## 一、Redis特点|应用场景

### 1）Redis特点

1. 基于内存，并且支持持久化到磁盘。
2. 基于key-value的非关系型数据库。
   * 关系型：`Oracle、MySQL、MariaDB`
   * 非关系型：`Redis、MongoDB`
3. 数据类型丰富：五大数据类型（**字符串、列表、哈希散列、集合、有序集合**）。
4. 核心命令是单进程单线程。

### 2）应用场景

1. 作为缓存。
2. 并发计数。
3. 排行榜（**有序集合**）。
4. 任务队列（**列表：生产者消费者模型**）。
5. 共同关注、共同好友（**集合**）。

## 二、关于Redis

### 1）启动和连接

<font color=red>**Redis默认端口号：6379**</font>

* 服务端启动：`sudo /etc/init.d/redis-server start|stop|restart|status`
* 客户端连接：`redis-cli -h IP地址 -p 端口号 -a 密码`

### 2）配置文件和常规配置

* 配置文件：`/etc/redis/redis.conf`

* 常规配置

  <font color=red>**任何服务只要更新了配置文件的相关配置，则一定要重启服务才能生效。**</font>

  * 设置密码：requirepass 密码

  * 远程连接

    `1. # bind 127.0.0.1 ::1`

    `2. protected_mode no`

### 3）服务启动文件|配置文件|日志文件

#### 1）启动文件

位置：/etc/init.d/....

```python
1.mysql:  /etc/init.d/mysql start|stop...
2.redis:  /etc/init.d/redis-server start|stop...
3.nginx:  /etc/init.d/nginx start|stop...
```

#### 2）配置文件

服务配置文件：/etc/服务名/

```python
1.mysql: /etc/mysql/mysql.conf.d/mysqld.cnf
2.redis: /etc/redis/redis.conf
3.nginx: /etc/nginx/nginx.conf
```

#### 3）日志文件

<font color=red>**有问题，看日志：/var/log/...**</font>

```python
1.mysql: /var/log/mysql/xxx.log
2.redis: /var/log/redis/redis-server.log
3.nginx: /var/log/nginx/xxx.log
```

## 三、Redis命令

### 1）通用命令

1. 切换库：`SELECT index`
2. 查看所有key：`KEYS *`
3. 删除key：`DEL key`
4. 清除当前库：`FLUSHDB`
5. 清除所有库：`FLUSHALL`
6. 判断key是否存在：`EXISTS key`
7. 查看存活时间：`TTL key`

### 2）字符串

<font color=red>**应用场景：常规计数(点赞数、评论数)、缓存热点数据响应(热门文章|热销商品|网站首页)、存储过期数据(短信验证码)**</font>

1. set key value nx ex 时间

2. get key
3. incr  key
4. decr  key
5. incrbyfloat key

### 3）列表

<font color=red>**应用场景：任务队列|消息队列，生产者消费者模型，Redis充当消息队列的角色，用来存储任务（生产者只要有任务，会推送到LPUSH任务队列Redis中，消费者会从消息队列中获取任务BRPOP并执行）。**</font>

1. LPUSH   key  element

2. RPUSH  key   element

3. BLPOP  key

4. BRPOP  key

5. LPUSH + BROPOP组合，经常用于生产者消费者模型，Redis列表充当任务队列的角色！

6. LTRIM key start stop 

   <font color=red>比如：保留微博评论的前500条数据。</font>

### 4）哈希散列

<font color=red>**应用场景：存储对象类型数据（对象的ID作为key，属性名作为Field，属性值作为value）**</font>

<font color=blue>**用户不同维度数据统计，比如关注数、粉丝数、作品数、点赞数（以用户的ID作为Redis中key，不同维度作为Field，具体的值作为value）。**</font>

<font color=green>**存储购物车中数据（用户ID作为key，商品ID作为Field，商品数量和选中状态作为value）**</font>

```python
1.用户ID：user_id
2.商品ID: product_id
3.商品数量: count
4.商品选中状态: selected

# ID为1的用户，加了商品ID为6的到购物车，加了3个，是选中的状态
#            加了商品ID为8的到购物车，加了5个，是选中的状态
#            加了商品ID为2的到购物车，加了1个，是未选中的状态

{
    "carts_1": {
        "6": [3, 1],
        "8": [5, 1],
        "2": [1, 0],
    } 
}
```

1. HSET key field value field value ...
2. HGET key field
3. HGETALL key
4. HLEN key
5. HINCR  key  field increment

### 5）集合

<font color=red>**应用场景1：利用集合交集获取共同关注、共同好友！**</font>

<font color=green>**应用场景2：增量爬虫，利用集合存储每个请求的指纹！利用集合sadd()的返回值来判断是否为新更新的网页，返回值为1则说明为新更新的网页，返回值为0说明之前已经抓取过。**</font>

1. SADD key member
2. SMEMBERS key
3. SPOP key [count]
4. SCARD key
5. SINTERSTORE  destination  key1  key2

### 6）有序集合

<font color=red>**应用场景：各种排行榜。**</font>

<font color=black>**有序指的是升序或者降序，有序集合中的每个成员都会关联一个浮点数的分值。**</font>

1. ZADD  key  score  member  score member ... ...
2. ZRANGE  key  start stop  withscores
3. ZREVRANGE key start stop withscores
4. ZREVRANK key member  `查看某个成员排名`
5. ZUNIONSTORE  destination  numkeys  key1  key2  weights  xx xx  aggregate  SUM|MAX|MIN  `并集`
6. ZINTERSTORE  destination  numkeys  key1  key2  weights  xx xx  aggregate  SUM|MAX|MIN `交集`

7. 练习

   * 查看当前库中所有的key

     `KEYS *`

   * 创建key（字符串）： sms_13603263409       871016

     `SET sms_13603263409  871016`

   * 创建集合并查看（普通集合）：myset_eg    

     `SADD myset_eg xx yy`

     `SMEMBERS myset_eg`

   * 创建有序集合  并升序查看  |  降序查看

     stu_salary     Lucy - 8000，Tom - 12000， Jack - 6000

     `ZADD stu_salary 8000 Lucy 12000 Tom 6000 Jack`

     `ZRANGE stu_salary 0 -1 withscores`

     `ZREVRANGE stu_salary 0 -1 withscores`

   * 查看有序集合中成员的总数量

     `ZCARD stu_salary`

   * 查看Tom的薪资排名

     `ZREVRANK stu_salary Tom`

## 数据持久化

### 1）意义

​		为了数据安全。

​		数据持久化是将内存中的数据备份到磁盘一份，用于数据恢复；当Redis服务重启或者服务器重启时，Redis会自动将磁盘中的数据快速恢复到内存中一份。

### 2）数据持久化方式

#### 2.1 RDB模式

* 全量备份数据

* 每次全量备份，消耗更多的资源，而且备份频率不能太高

* 数据恢复效率高

* 相关策略

  **/var/lib/redis/dump.rdb**

  ```python
  # 三种策略满足其中任何一个策略时，会自动触发 BGSAVE 执行全量备份
  # 时间过去900秒，并且数据库中的数据发生一次以上更改时，会自动进行备份
  save 900 1
  save 300 10
  save 60 10000
  ```

#### 2.2 AOF模式

* 增量备份数据
* 每次只备份少量命令，更加高效
* 数据恢复效率相对较低，因为要执行所有的Redis命令

* 相关策略

  **/var/lib/redis/appendonly.aof**

  ```python
  appendfsync always    # 不丢失任何数据,相对比较消耗资源,数据安全！
  appendfsync everysec  # 最多丢失1秒钟数据
  appendfsync no        # 丢失数据的数量不一定，由操作系统决定
  ```

<font color=red>**数据恢复时，如果两种模式都开启，则默认走AOF模式恢复。**</font>

### 持久化操作步骤

**开启AOF数据持久化方式**

```python
1.sudo su
2.gedit /etc/redis/redis.conf
  1094行:  appendonly yes  # 将no改为yes
3.保存退出
4.重启redis服务: /etc/init.d/redis-server restart
5.确认备份文件: cd /var/lib/redis/   # 有一个appendonly.aof
6.到Redis命令行任意敲几条增删改指令，然后到 appendonly.aof 文件中确认
  cat appendonly.aof
  gedit appendonly.aof
  vim appendonly.aof
```













