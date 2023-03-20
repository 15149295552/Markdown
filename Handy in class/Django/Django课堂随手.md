[TOC]

# DJANGO-DAY01

## 一、介绍

### 1）第三阶段课程介绍

* 课程内容（<font color=red>19个工作日</font>）
  1. DJANGO框架（<font color=red>6个工作日左右</font>）
  2. Redis数据库（<font color=red>2个工作日左右</font>）
  3. Ajax（<font color=red>1个工作日左右</font>）
  4. 综合项目（<font color=red>10个工作日左右</font>）
* 目的
  1. 从事Python开发工程师岗位
  2. 其他岗位附加技能
  3. 提升编码能力，培养编程思维

* 特点

  抽象、知识点多、代码量大

## 二、Python就业岗位

* Python开发工程师
* 人工智能工程师
* 数据分析工程师（**不好从事**）
* 爬虫工程师（**TTS赠送爬虫视频**）
* 自动化测试工程师（**补充相关测试内容**）
* 自动化运维工程师（**不好从事**）

## 三、DJANGO项目

### 1）项目流程

* 创建并启动项目

  `1.django-admin startproject 项目名`

  `2.cd 项目名`

  `3.python3 manage.py runserver`

* 全局配置settings.py

  `1.LANGUAGE_CODE = "zh-Hans"`

  `2.TIME_ZONE = "Asia/Shanghai"`

* 编写路由urls.py

  ```python
  from django.urls import path
  from . import views
  
  urlpatterns = [
      path("xxx/xxx", views.函数名),
  ]
  ```

* 完成视图views.py

  ```python
  from django.http import HttpResponse
  
  def xxx_view(request):
     	return HttpResponse("xxx")
  ```

* 浏览器测试

## 2）项目目录结构

django-admin startproject 项目名

```python
项目目录
  |--manage.py # 负责启动项目、创建应用、数据库...
  |--同名目录
      |--settings.py # 全局配置
      |--urls.py     # 路由
      |--views.py    # 视图:负责业务逻辑处理
      |--models.py   # 模型:负责和数据库交互
      |--wsgi.py     # 项目部署
      |--asgi.py     # 项目部署
```

### 3）settings.py中常见配置

1. BASE_DIR: `项目目录的绝对路径`
2. DEBUG: `开发模式DEBUG=True`
3. ALLOWED_HOSTS: `允许的请求头host值`
4. INSTALLED_APPS: `应用列表`
5. TEMPLATES: `模板文件,存放html`
6. DATABASES: `配置数据库`
7. LANGUAGE_CODE: `语言，zh-Hans` 
8. TIME_ZONE：`时区,Asia/Shanghai`

### 4）path转换器

* 作用

  将URL地址的数据按照关键字传参的方式传给视图函数。

* 语法格式

  `<类型:自定义变量名>`

* 示例

  * URL: `http://127.0.0.1:8000/user/liying`

  * urls.py

    `path("user/<str:username>", views.user_view)`

  * views.py

    ```python
    def user_view(request, username)
    	return HttpResponse("xxx")
    ```

* 常见的path转换器

  * str转换器：`<str:自定义变量名>`
  * int转换器：`<int:自定义变量名>`

### 5）常见服务端口号

* MySQL：3306
* http：80
* https：443
* django项目默认端口号：8000

## 四、HTTP请求和响应

### 1）请求内容

* 起始行：`请求方法、请求路由、使用协议`

* 请求头

  * Accept：浏览器希望获取什么类型数据
  * Content-Type：指明请求体数据类型，text/html、text/plain、application/json。

* 请求体

  前端需要交给服务器端数据，可以是Form表单形式提交，也可以是json形式提交。

### 2）请求方法

* GET：`在服务器端获取资源`
* POST：`在服务器端新增资源`
* PUT：`在服务器端更新资源`
* DELETE：`在服务器端删除资源`

<font color=red>**四种请求对应了对服务器端资源的增删改查四种操作，这只是一套指导规范，具体以入职以后的公司风格为准。**</font>

### 3）HttpRequest请求对象

* 说明

  视图函数的第一个参数是HttpRequest请求对象，服务器端收到请求后，会根据请求数据报文创建请求对象。

* 请求对象属性

  |  属性  |            作用            |  数据类型  |
  | :----: | :------------------------: | :--------: |
  | method |          请求方法          |   字符串   |
  |  GET   |         查询字符串         | QueryDict  |
  |  POST  | 请求体数据：以表单形式提交 | QueryDict  |
  |  body  | 请求体数据：以json形式提交 | 字节串:b"" |
  |  META  |         请求头数据         |  字典:{}   |

  注意：<font color=green>**request.META: Django会把所有请求头都大写，并加上HTTP_前缀，比如：HTTP_ACCEPT、HTTP_USER_AGENT等。也可以获取客户端的IP地址：REMOTE_ADDR**</font>

### 4）关于响应

#### 4.1 响应对象HttpResponse

-  视图函数中

  return  HttpResponse(content="响应体", content_type="类型", status=自定义状态码)

  content_type示例：

  * html格式：`text/html;charset=utf-8`
  * 文本格式：`text/plain;charset=utf-8`
  * json格式：`application/json;charset=utf-8`

#### 4.2 目前遇到的响应状态码

* 404：Page Not find，检查路由和浏览器URL地址是否匹配。

* 403：403 Forbidden，csrf校验未通过，关闭csrf中间件。

  ​		  settings.py中MIDDLEWARE： csrf中间件注释掉。

* 500：服务器错误，检查视图函数。

# DJANGO-DAY02

## 一、设计模式

<font color=red>**所有设计模式的最终作用都是为了降低模块间的耦合度。**</font>

### 1）MVC

* M：模型层model，封装数据库层。
* V：视图层View，展示数据。
* C：控制器层Controller,<font color=red>**核心**</font>，处理用户请求、获取数据、返回结果。

### 2）MTV

* M：模型层model，封装数据库层。
* T：模板层Templates，展示数据。
* V：视图层View，<font color=red>**核心**</font>，处理用户请求、获取数据、返回结果。

## 二、模板层Template

### 1）定义

​	可以根据视图中传递的字典数据动态生成相应的HTML网页，并呈现给用户。

### 2）使用流程

* 项目目录或者应用目录下创建模板目录：templates

* settings.py中配置模板配置项（**TEMPLATES**）

  1. BACKEND：指定模板引擎（<font color=red>**默认**</font>）
  2. DIRS：指定模板的搜索目录（<font color=red>**[BASE_DIR / "templates"]**</font>）
  3. APP_DIRS：是否在应用的templates目录中搜索模板文件（<font color=red>**默认**</font>）
  4. OPTIONS：有关模板的选项（<font color=red>**默认**</font>）

* 创建模板html文件

* 视图函数中render()方法加载模板

  ```python
  from django.shortcuts import render
  
  def xxx_view(request):
      return render(request, "html文件名", 字典数据)
  ```

### 3）模板语言

#### 3.1 模板传参

​	`return render(request, "xxx.html", 字典数据)`

#### 3.2 模板变量

* 语法格式：`{{  }}`
* 分类
  1. 普通变量：{{ 变量名 }}
  2. 列表中指定索引元素：{{ 变量名.index }}
  3. 获取自定中指定的key：{{ 变量名.key }}
  4. 获取指定函数返回值：{{ 变量名.函数名 }}
  5. 获取对象数据：{{ 对象名.方法 }}

#### 3.3 模板标签

* 作用：将一些服务器端的功能嵌入到模板中。

* 标签语法

  ```python
  {% 标签 %}
  ... ...
  {% end标签 %}
  ```

* if标签

  ```python
  {% if 表达式 %}
  	...
  {% elif 表达式 %}
  	...
  {% else %}
  	...
  {% endif %}
  ```

* for标签

  ```python
  {% for 变量名 in 可迭代对象 %}
  	...
  {% empty %}
  	...
  {% endfor %}
  ```

* 加载静态文件

  ```python
  {% load static %}
  <img src="{% static 'liying.png' %}"
  ```

## 三、静态文件配置

* settings.py中配置请求路由和寻址路径

  ```python
  STATIC_URL = "/static/"
  STATICFILES_DIRS = (BASE_DIR / "static",)
  ```

* 项目目录下创建static目录，并存放图片

* 浏览器测试

  `http://127.0.0.1:8000/static/liying.png`

## 四、应用APP

### 1）定义

​	应用是在django项目中独立的一个业务模块，每个应用包含自己的路由、视图、模型、模板。

### 2）使用流程

1. 创建应用：python3 manage.py startapp 应用名

2. 注册应用：settings.py 中 INSTALLED_APPS:  [应用名,]

3. 匹配主路由：<font color=red>**注意：include()方法**</font>

   ```python
   from django.urls import path, include
   
   path("主路由", include("应用名.urls"))
   ```

4. 匹配分布式路由：<font color=red>**单独创建urls.py**</font>

5. 完成视图

<font color=green>**注意：查找模板文件时，优先查找外层的templates目录下的模板文件，如果没有则按照settings.py中INSTALLED_APPS中应用注册的顺序依次逐个查找。**</font>

# DJANGO-DAY03

## 一、DJANGO配置MySQL

* 创建数据库

  `create database 库名 default charset utf8;`

* 配置数据库（<font color=red>**settings.py**</font>）

  <font color=red>**指定数据库引擎、主机地址、用户名、密码、端口号、具体库**</font>

  示例

  ```python
  DATABASES = {
      "default": {
          "ENGINE": "django.db.backends.mysql",
          "HOST": "MySQL数据库服务器的IP地址",
          "USER": "用户名",
          "PASSWORD": "密码",
          "NAME": "库名",
          "PORT": "3306",
      }
  }
  ```

  * 常见的数据库引擎

    ```python
    django.db.backends.mysql
    django.db.backends.sqlite3
    django.db.backends.oracle
    ... ...
    ```

## 二、ORM框架

### 1）定义

​	ORM，对象关系映射，避免使用SQL操作数据库，而是使用类和对象对数据库进行操作。

## 2）ORM特点

#### 2.1 优点

* 只需要面向对象编程，无须面向数据库编程。
* 实现数据模型和数据库的解耦，不用关心公司使用的数据库（**MySQL、Oracle等**）的细节，通过简单的配置就能实现更换项目数据库，无须修改大量代码。

#### 2.2 缺点

* 需要将对应的操作语句转为SQL命令再执行，映射的过程中会有一定程度的性能丢失。
* 对于复杂的业务使用成本较高。

### 3）ORM对应关系

|    ORM     |   数据库   |
| :--------: | :--------: |
|   一个类   |   一张表   |
| 一个类属性 | 一个表字段 |
|  一个对象  | 一条表记录 |

得到对象：

1. user = UserProfile.objects.create(username="xxx", ........)

2. user_query = UserProfile.objects.filter(username="xxx")

   user = user_query[0]

### 4）ORM使用流程

1. 创建库

2. 配置数据库

3. 创建应用

   `python3 manage.py startapp 应用名`

4. 创建模型类

   ```python
   from django.db import models
   
   class Xxx(models.Model):
       username = models.CharField(...)
   ```

5. 迁移同步

   python3 manage.py makemigrations

   python3 manage.py migrate

### 5）迁移文件混乱的处理方法

1. 删除库：`drop database 库名;`

2. 建库：`create database 库名 default charset utf8;`

3. 删除迁移文件：`删除所有应用下的migrations中的 000_xxx... 的文件`

4. 迁移同步

   `python3 manage.py makemigrations`

   `python3 manage.py migrate`

### 6）字段类型

* 类型

  |    models.py    |     mysql数据库     |    示例     |
  | :-------------: | :-----------------: | :---------: |
  | BooleanField()  |     tinyint(1)      | 是否会员... |
  |   CharField()   |     varchar(20)     |             |
  | DateTimeField() | datetime、timestamp | 注册时间... |
  | DecimalField()  |    decimal(M,D)     |   单价...   |
  |  EmailField()   |      varchar()      |    邮箱     |
  | IntegerField()  |         int         |   销量...   |

* 字段类型需要注意

  1. 字符类型必须指定宽度：**CharField(max_length=宽度)**

  2. 浮点型必须指定总位数和小数位位数：**DecimalField(max_digits=10, decimal_places=2)**

  3. 日期时间类型设置自动创建或者更新时间：

     **auto_now_add=True: 插入数据时自动创建时间**

     **auto_now=True: 增删改时会自动更新时间**

  4. 存储邮箱：**EmailField()**

* 字段选项注意

  1. 主键：primary_key

     <font color=red>模型类未指定主键，则Django会自动创建id字段并设置为主键；如果模型类指定主键，则Django不会创建id字段。</font>

  2. 索引：db_index

     <font color=red>在经常用来查询的字段上建立索引，也是数据库优化的一种方式。</font>

  3. 唯一：unique

     <font color=red>字段的值不允许重复。</font>

### 7）内部类Meta

​	使用Meta内部类，可以对模型类赋予属性，比如指定表名、指定admin后台数据格式显示等等。



# DJANGO-DAY04

## 一、ORM增删改查

<font color=blue>**统统使用管理器对象objects，每个继承models.Model的模型类都会同样继承objects。**</font>

### 1）增-create

* 方法：`对象名 = 模型类.objects.create(属性=值,属性=值, ...)`

* 返回值

  创建好的实体对象，可以通过：对象名.类属性 获取指定字段的值

* 示例

  ```python
  user = UserProfile.objects.create(username="赵丽颖")
  user.username
  user.password
  user.created_time
  user.updated_time
  ```

### 2）查(get|filter|all)

#### 2.1 get()

* 作用：查找满足条件的唯一一条数据

* 方法：`模型类.objects.get(条件1,条件2,...)`

* 返回值：查询成功则返回数据对象，查询失败则会抛出异常。

  <font color=purple>视图中只要用到get()方法查询，则一定要加try语句。</font>

* 示例

  ```python
  try:
      user = UserProfile.objects.get(username="赵丽颖")
  except Exception as e:
      print("查询异常:", e)
      pass
  ```

#### 2.2 all()

* 作用：查询表中所有数据

* 返回值：<QuerySet [数据对象1，数据对象2]>

  <font color=purple>此方法不抛出异常，如果未查到数据则返回空的<QuerySet []></font>

* 示例

  ```python
  user_query = UserProfile.objects.all()
  for user in user_query:
      pass
  ```

#### 2.3 filter()

* 作用：查询满足条件的一组数据

* 返回值：<QuerySet [数据对象1,数据对象2]>

  <font color=purple>不会抛出异常，未查到数据返回空的<QuerySet []></font>

* 示例

  ```python
  user_query = UserProfile.objects.filter(...,...)
  # 使用场景1：遍历
  for user in user_query:
      pass
  
  # 使用场景2:判断
  if not user_query:
      pass
  else:
      pass
  ```

### 3）改

#### 3.1 单个对象修改

* 步骤：一查二改三保存

* 示例

  ```python
  # 方式1
  user = UserProfile.objects.get(id=1)
  user.password = "新密码"
  user.save()
  
  # 方式2
  user_query = UserProfile.objects.filter(id=1)
  user = user_query[0]
  user.password = "新密码"
  user.save()
  ```

#### 3.2 批量修改

* 方法：`<QuerySet [...,..]>.update(x=新值,y=新值)`

* 示例

  ```python
  result = UserProfile.objects.filter(id__gt=3).update(x="新值", y="新值")
  
  # 返回值result：返回受影响的数据的条数,如果filter()未查到任何数据则返回值为0
  ```

### 4）删

* 单个对象或者<QuerySet []>直接调用delete()方法

* 步骤：一查二删除

* 示例

  ```python
  UserProfile.objects.get(id=1).delete()
  UserProfile.objects.filter(id__lte=8).delete()
  
  # 生产环境中，多数使用伪删除
  # 不会真正从数据表中 delete() 删除数据，而是采用一个伪删除字段，比如：is_delete=True|False
  ```

## 二、关于查询

### 1）常用方法

#### 1.1 values()

* 示例：`query = Address.objects.values("receiver", "mobile")`
* 返回值：`<QuerySet [{"receiver": "xx", "mobile": "xx"},{}]>`

#### 1.2 values_list()

* 示例：`query = Address.objects.values_list("receiver", "mobile")`
* 返回值：`<QuerySet [("xxx", "xxx"),(),...]>`

#### 1.3 order_by()

* 作用：排序（升序|降序）

* 返回值：`<QuerySet []>`

* 示例

  ```python
  # 1.升序:按照销量升序排列
  query = Product.objects.order_by("sales")
  # 2.降序:按照库存降序排列
  query = Product.objects.order_by("-stock")
  ```

### 2）查询谓词

<font color=red>所有的查询谓词均用于查询条件。</font>

1. __in

   ```python
   query = UserProfile.objects.filter(id__in=[1,3,5])
   query = UserProfile.objects.filter(id__in=(1,3,5))
   query = UserProfile.objects.filter(id__in={1:x, 2:x, 5:x})
   query = UserProfile.objects.filter(id__in=<QuerySet [1,3,5]>)
   ```

2. __contains

3. `__gt、__gte、__lt、__lte`

4. 练习:

   1. 查询Book表中price大于等于50的信息

      `books = Book.objects.filter(price__gte=50)`

   2. 查询Author表中姓王的人的信息

      `authors = Author.objects.filter(name__startswith="王")`

   3. 查询Author表中Email中包含"w "的人的信息

      `authors = Author.objects.filter(email__contains="w")`

   4. 将Book表中的数据按照零售价格(market_price)降价排序

      `books = Book.objects.order_by("-market_price")`

   5. 查询用户表UserProfile中的所有数据

      `users = UserProfile.objects.all()`

   6. 查询用户表中id为5，并且用户名为zhaoliying的数据

      `user = UserProfile.objects.get(id=5,username="zhaoliying")`

   7. 查询Book表中价格>=30，并且是清华大学出版社出版的图书

      `books = Book.objects.filter(price__gte=30,pub="清华大学出版社")`

   8. 查询author表中，年龄<30的数据

      `authors = Author.objects.filter(age__lt=30)`

   9. 将Author表中吕老师的邮箱调整为：309435365@qq.com

      `author = Author.objects.get(name="吕老师")`

      `author.email = "309435365@qq.com"`

      `author.save()`

   10. 将Book表中HTML5这本书的出版社改为：人民教育出版社

       `book = Book.objects.get(title="HTML5")`

       `book.pub = "人民教育出版社"`

       `book.save()`


### 3）分组聚合

#### 3.1 聚合

* 方法：aggregate()

* 示例

  ```python
  from django.db.models import Avg, Max, Min, Count, Sum
  
  result = Book.objects.aggregate(avg_price=Avg("price"))
  # result: {"avg_price": Decimal(88.88)}
  ```

#### 3.2 分组聚合

* 方法：annotate()

* 步骤

  * 获取分组聚合的咧
  * 使用对应的方法进行分组聚合

* 示例

  ```python
  pub_set = Book.objects.values("pub")
  result = pub_set.annotate(my_count=Count("pub"))
  # result: <QuerySet [{"pub": "xxx", "my_count": xx}]>
  ```

### 4）F对象

<font color=blue>一个F对象代表数据表中某个字段的相关信息。</font>

* 作用

  通过对数据中字段的值在不单独获取的情况下进行操作。

* 示例

  ```python
  from django.db.models import F
  
  Book.objects.all().update(price=F("price")+10)
  Book.objects.filter(market_price__gt=F("price"))
  ```

### 5）Q对象

* 作用：`用于查询时的复杂条件，比如：逻辑或|、逻辑非~`

* 语法

  ```python
  from django.db.models import Q
  
  Q(条件1) | Q(条件2)
  Q(条件1) & Q(条件2)
  Q(条件1) | ~Q(条件2)
  ```

## 三、DJANGO使用原生SQL

### 1）raw()

* 方法：`模型类.objects.raw("sql语句", [拼接参数])`

* 注意

  <font color=green>**raw()方法要使用拼接参数传递数据，避免sql注入问题。**</font>

* sql注入

  <font color=green>**SQL注入是指web应用程序对用户输入的数据的合法性没有判断或者顾虑不严，攻击者可以在Web应用程序中事先定义好的查询语句的末尾添加额外的SQL语句，在管理员不知情的情况下实现非法操作，以此实现欺骗数据库服务器执行非授权的任意查询，从而进一步获取相应的数据信息。**</font>

# DJANGO-DAY05

## 一、Admin后台数据库管理

### 1）作用

​	使用Django Admin可以快速对数据库中的各个数据表进行增删改查。将所有需要管理的模型（数据表）集中在一个平台，不仅可以选择性的管理模型，还可以快速定制数据条目查询、过滤和搜索条件。

### 2）使用流程

1. 创建超级用户superuser：`python3 manage.py createsuperuser`

2. 注册模型类admin.py（可以自定义样式）

   ```python
   from django.contrib import admin
   
   class XxxManager(admin.ModelAdmin):
      	# 自定义数据表显示项
       list_display = []
       list_display_links = []
       list_editable = []
       search_files = []
       
   admin.site.register(模型类, XxxManager)
   ```

3. admin管理页面操作

   `http://127.0.0.1:8000/admin/`

## 二、关系映射

### 1）说明

1. 一对一：`外键属性 = models.OneToOneField(模型类,on_delete=级联动作)`

   <font color=red>外键在任何一个模型类都可以。</font>

2. 一对多：`外键属性 = models.ForeignKey(模型类,on_delete=级联动作)`

   <font color=red>外键必须在多的模型类中。</font>

3. 多对多：`外键属性 = models.ManyToManyField(模型类)`

   <font color=red>外键在任何一个模型类都可以。</font>

### 2）正向查询和反向查询

#### 2.1 说明

1. 正向查询：通过有外键的模型类对象 查找另一个模型类数据。
2. 反向查询：通过没有外键的模型类对象 查找另一个模型类数据。

#### 2.2 正向查询

<font color=red>**第一步查找有外键的模型类数据对象，比如叫：obj**</font>

1. 一对一：`obj.外键属性  (obj.外键属性.name | obj.外键属性.price)`

2. 一对多：

   ```python
   1. 某本书出版社是哪个？
   obj = Book.objects.get(name="某本书")
   obj.pub    
   obj.pub.name
   
   2.某出版社出版过哪些图书
   Book.objects.filter(pub="某出版社的数据对象")
   ```

3. 多对多

   ```python
   1.笑傲江湖的作者
   obj = Book.objects.get(name="笑傲江湖")
   obj.外键属性.all()
   
   2.吕泽写过哪些书？
   Book.objects.filter(author="吕泽的数据对象")
   ```


#### 2.3 反向查询

<font color=red>**第一步查找没有外键的模型类数据对象，比如叫：obj**</font>

1. 一对一：`obj.另一个类名小写`
2. 一对多：`obj.另一个类名小写_set.all()`
3. 多对多：`obj.另一个类名小写_set.all()`

## 三、COOKEIS

因为HTTP协议是无状态的，所以无法保持客户端和服务端的会话状态。

### 1）作用

保持客户端和服务器的会话状态。

### 2）特点

1. 保存在客户端浏览器的本地存储中。
2. 数据以键值对的方式存储。
3. 按域存储隔离，不同域之间无法访问。
4. 不要太大，会降低响应速度。

### 3）常用方法

通过HttpResponse对象进行cookies的设置、修改、删除。

**resp = HttpResponse("xxx")**

1. 设置和修改：resp.set_cookie(key, value, max_age)
2. 删除：resp.delete_cookie(key)



**<font color=red>在浏览器中，Cookie是服务器让浏览器帮忙存储和携带信息的手段，浏览器会存储它，并且在后续的HTTP请求中再次发送给服务器。</font>**



## 四、session

<font color=red>**因为HTTP协议是无状态的，所以无法保持服务端和客户端的对话。**</font>

### 1）作用

​	保持客户端和服务器端的会话状态。

​	在服务器端开辟一段空间（**django_session表**）用于保留浏览器和服务器交互时的重要数据。

### 2）cookies和session原理图

![](./imgs/02_COOKIES_SESSION.png)

### 3）DJANGO实现session会话保持

1. settings.py中添加session应用及session中间件

   ```python
   1) INSTALLED_APPS 中添加 'django.contrib.sessions'
   2) MIDDLEWARE 中添加 'django.contrib.sessions.middleware.SessionMiddleware'
   ```

2. 视图中保存session(django_session表)

   request.session[key] = value

   ![](./imgs/05_django_session.png)

3. 视图中获取session

   request.session.get(key)

### 4）DJANGO常用配置

1. SESSION_COOKIE_AGE

   - 作用: 指定sessionid在cookies中的保存时长(默认是2周)，如下:

     `SESSION_COOKIE_AGE = 60 * 60 * 24 * 7 * 2`

2. SESSION_EXPIRE_AT_BROWSER_CLOSE = True
   设置只要浏览器关闭时,session就失效(默认为False)  

### 5）清除过期的session数据

​	因为django_session表是 单表设计，可能还会存在浏览器故意删掉sessionid 以及 过期数据未删除导致该表数据量持续增持给服务器带来存储压力和查询压力，可以每晚执行 python3 manage.py clearsessions 删除已过期的session数据。

# DJANGO-DAY06

## 一、缓存

### 1）作用

​	因视图渲染资源及成本消耗较高，所以可将用户访问频率较高的数据存入缓存中；

​	用户访问时先从缓存中搜索是否存在所需数据，如存在则直接返回给用户，如不存在则到数据库服务器中查找并缓存到指定缓存中。

​	应用场景比如：博客列表页、电商网站商品详情页等。

### 2）DJANGO中设置缓存

#### 2.1 步骤

1. 配置缓存配置项settings.py

2. 配置缓存配置项CACHES

   - 引擎、指定缓存区域、缓存时间、缓存最大条数、清理缓存

   ```python
   CACHES = {
       'default': {
           'BACKEND': 'django.core.cache.backends.db.DatabaseCache',
           'LOCATION': 'my_cache_table',
           'TIMEOUT': 300,  #缓存保存时间 单位秒，默认值为300, 
           'OPTIONS':{
               'MAX_ENTRIES': 300, #缓存最大数据条数
               'CULL_FREQUENCY': 2,#缓存条数达到最大值时 删除1/x的缓存数据
           }
       }
   }
   ```

3. 创建缓存表：`python3 manage.py createcachetable`

   ![](./imgs/06_cache_table.png)

4. 直接使用缓存装饰器实现

   <font color=red>**from django.views.decorators.cache import cache_page**</font>

   - 视图：@cache_page(30)

     ​            def xxx_view(request):...

   - 路由：path("", cache_page(30)(views.xxx_view))

#### 2.2 常用的缓存引擎

1. Redis缓存引擎："BACKEND": "django_redis.cache.RedisCache"
2. 数据库缓存引擎： 'BACKEND': 'django.core.cache.backends.db.DatabaseCache'
3. 文件系统缓存引擎：'BACKEND': 'django.core.cache.backends.filebased.FileBasedCache'
4. 本地内存引擎：'BACKEND': 'django.core.cache.backends.locmem.LocMemCache'

### 3）缓存API

- 作用：`局部缓存部分结果。`

- 使用

  ```python
  from django.core.cache import caches
  
  caches["default"].set(key, value, expire)
  caches["code"].get(key)
  ```

- 常用方法

  1. 设置缓存键值对

     caches["default"].set(key, value, expire)

  2. 获取缓存中的指定数据

     caches["default"].get(key)

  3. 删除缓存中指定key

     caches["default"].delete(key)

## 二、中间件

### 1）定义

​	中间件是 Django 请求/响应处理的钩子框架，<font color=red>用于全局改变 Django 的输入或输出。</font>

### 2）中间件分类及原理图

![](./imgs/04_中间件.png)

### 3）常用中间件及方法

<font color=red>**中间件类须继承自 `django.utils.deprecation.MiddlewareMixin`类**</font>

1. 处理请求中间件

   - 方法：`def process_request(self, request):`

   - 说明：所有请求在<font color=red>执行路由之前</font>此方法都会被调用。

   - 返回值

     None：说明请求校验通过，进入处理视图中间件。

     HttpResponse：请求校验未通过，返回响应直接结束。

   - 应用场景：IP地址访问次数校验。

2. 处理视图中间件

   - 方法：`def process_view(self, request, callback, callback_args, callback_kwargs):`

   - 说明：所有请求在<font color=red>调用视图之前</font>此方法都会被调用。

   - 返回值

     None：说明请求校验通过，进入处理响应中间件。

     HttpResponse：请求校验未通过，返回响应直接结束。

   - 应用场景：csrf校验。

3. 处理响应中间件

   - 方法：`def process_response(self, request, response):`

   - 说明：所有请求在<font color=red>返回响应之前</font>此方法都会被调用。

   - 返回值

     HttpResponse：返回处理后的HttpResponse。

   - 应用场景：敏感词的屏蔽。

4. 处理异常中间件

   - 方法：`def process_exception(self, request, exception):`

   - 说明：当中间件处理过程中抛出异常时调用。

   - 返回值

     HttpResponse对象。

5. 处理render()返回中间件

   - 方法：`def process_template_response(self, request, response):` 

   - 说明：在视图函数执行完毕且试图返回的对象中包含render方法时被调用。

   - 返回值

     实现了render方法的响应对象

### 4）实现中间件

1. 编写中间件类

   ```python
   from django.http import HttpResponse
   from django.utils.deprecation import MiddlewareMixin
   
   class MyMiddleWare(MiddlewareMixin):
       def process_request(self, request):
           print("中间件方法 process_request 被调用")
   
       def process_view(self, request, callback, callback_args, callback_kwargs):
           print("中间件方法 process_view 被调用")
   
       def process_response(self, request, response):
           print("中间件方法 process_response 被调用")
           return response
   ```

2. 注册中间件

   ```python
   MIDDLEWARE = ['应用名.py文件名.中间件类名',]
   ```

3. 浏览器访问任意路由测试

4. 中间件官方原理图

   ![](./imgs/07_middleware.jpeg)



## 三、Django发送邮件

### 1）说明

​	框架在在 django.core.mail 模块中内置了邮件发送功能，通常情况下发送邮件需要使用 SMTP 服务，常用的免费邮箱服务器有 163、126、QQ 等

### 2）实现步骤

#### 2.1 将QQ邮箱授权给DJANGO

​	登录QQ邮箱<https://mail.qq.com/>并修改设置

​	修改 `QQ邮箱->设置->帐户->“POP3/IMAP......服务->开启”`

​	<font color=red>手机发送短信并获取QQ邮箱授权码</font>

#### 2.2 配置DJANGO服务器

<font color=red>**settings.py**</font>

```python
# 1.固定写法
EMAIL_BACKEND = 'django.core.mail.backends.smtp.EmailBackend'
# 2.腾讯QQ邮箱 SMTP 服务器地址
EMAIL_HOST = 'smtp.qq.com'
# 3.SMTP服务的端口号
EMAIL_PORT = 25  
# 4.发送邮件的QQ邮箱
EMAIL_HOST_USER = 'xxxx@qq.com'  
# 5.在QQ邮箱->设置->帐户->“POP3/IMAP......服务” 里得到的在第三方登录QQ邮箱授权码
EMAIL_HOST_PASSWORD = '******'  
# 6.与SMTP服务器通信时，是否启动TLS链接(安全链接)默认false
EMAIL_USE_TLS = True  
```

#### 2.3 视图函数调用接口发送邮件

```python
from django.core import mail

mail.send_mail(
    subject,  #题目
    message,  # 消息内容
    from_email,  # 发送者[当前配置邮箱]
    recipient_list=['xxx@qq.com'],  # 接收者邮件列表
)
```



# 补充内容

### 1）md5加密

- 说明

  md5加密算法是哈希散列算法的一种，用于对明文字符进行加密，使数据信息更加安全。

- 哈希算法三大特性

  - 不可逆：密文无法解析为明文。
  - 定长输出：md5加密结果长度固定为32位，sha1加密结果长度固定为40位等。
  - 雪崩效应：加密字符串略有不同加密结果完全不同。

- 使用示例

  ```python
  import hashlib
  
  m = hashlib.md5()
  m.update(字符串.encode())
  m.hexdigest()
  # 加密结果为字符串，长度为32位。
  ```

### 2）base64编码

- 说明

  base64不是加密，是编码，亦可解码。

- 常用方法

  - 编码：base64.b64encode(b"")
  - 解码：base64.b64decode(b"")

- 使用示例

  ```python
  import base64
  
  # 1.编码
  result = base64.b64encode(b"").decode()
  
  # 2.解码
  result = base64.b64decode(b"").decode()
  ```

### 3）项目流程

* 产品经理：需求文档
* UI原型图设计
* 数据库设计（**ER图**）
* 开会：前端工程师、后端工程师
  ```python
  1.每个请求的路由
  2.请求方法
  3.请求体数据
  4.响应内容
  你是一个前端开发工程师，你在写注册功能：
  $.ajax({
      url: '',
      type: 'post',
      data: {'uname': xxx, 'pwd': xxx},
      success: function(resp){
          xxx
      }
  })
  ```
* 接口文档：`每个请求路由、请求方法、请求体、响应格式`
* 前后端并行开发
* 前后端联调
* 测试阶段
* 上线运行维护

### 4）DJANGO常用命令总结

**<font color=red>ORM: 对象关系映射</font>**

1. django-admin startproject 项目名
2. python3 manage.py runserver

3. python3 manage.py startapp 应用名

4. python3 manage.py makemigrations

5. python3 manage.py migrate

6. python3 manage.py shell
7. python3 manage.py createsuperuser
8. python3 manage.py createcachetable









