#### 1.短信验证码：有效期为5分钟

#### 2.您和xxx有几个共同好友，有几个共同关注

#### 3.点赞数、评论数、收藏数

#### 4.歌曲排行榜

#### 5.增量爬虫、分布式爬虫

#### 6.缓存





* 服务启动文件：/etc/init.d/....

  ```python
  1.mysql:  /etc/init.d/mysql start|stop...
  2.redis:  /etc/init.d/redis-server start|stop...
  3.nginx:  /etc/init.d/nginx start|stop...
  ```

* 服务配置文件：/etc/服务名/

  ```python
  1.mysql: /etc/mysql/mysql.conf.d/mysqld.cnf
  2.redis: /etc/redis/redis.conf
  3.nginx: /etc/nginx/nginx.conf
  ```

  



* 备份redis配置文件

  ```python
  sudo su
  cd /etc/redis/
  cp -p redis.conf redis.conf.bak
  ```

* 如果redis无法启动，则恢复配置文件

  ```python
  rm -rf redis.conf
  cp -p redis.conf.bak redis.conf
  /etc/init.d/redis-server restart
  ```





通用命令



* 字符串：增删改查
* 列表：增删改查
* 哈希：增删改查
* 集合：增删改查
* 有序集合：增删改查



* 字符串

  <font color=red>**应用场景：常规计数(点赞数、评论数)、缓存热点数据响应(热门文章|热销商品|网站首页)、存储过期数据(短信验证码)**</font>

  1. set key value nx ex 时间

  2. get key
  3. incr  key
  4. decr  key
  5. incrbyfloat key

* 列表

  <font color=red>**应用场景：任务队列|消息队列，生产者消费者模型，Redis充当消息队列的角色，用来存储任务（生产者只要有任务，会推送到LPUSH任务队列Redis中，消费者会从消息队列中获取任务BRPOP并执行）。**</font>

  1. LPUSH   key  element
  2. RPUSH  key   element
  3. BLPOP  key
  4. BRPOP  key
  5. LPUSH + BROPOP组合，经常用于生产者消费者模型，Redis列表充当任务队列的角色！
  6. LTRIM key start stop 

* 哈希散列

  <font color=red>**应用场景：存储对象类型数据（对象的ID作为key，属性名作为Field，属性值作为value）**</font>

  <font color=blue>**用户不同维度数据统计，比如关注数、粉丝数、作品书、点赞数（以用户的ID作为Redis中key，不同维度作为Field，具体的值作为value）。**</font>

  1. HSET key field value field value ...
  2. HGET key field
  3. HGETALL key
  4. HLEN key
  5. HINCR  key  field increment





```python
import pymysql

db = pymysql.connect(host="", user="", password="", database="", charset="")
cur = db.cursor()
cur.execute("", [])
db.commit()
cur.close()
db.close()
```



字符串：{"liying_fans": xxx, "liying_focus": xxx,... ...}   Redis需要管理多个key

哈希散列：{"trill_liying":  {"liying_fans": xxx, "liying_focus": xxx, ...}}





