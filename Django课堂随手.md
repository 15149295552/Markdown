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
  * int转化器：`<int:自定义变量名>`

### 5）常见服务器端口号

- MySQl:3306
- http:80
- https:443
- django项目默认端口号:8000

# 四、HTTP请求和响应

## 1）请求内容

* 起始行：`请求方法、请求路由、使用协议`
* 请求头
  * Accept：浏览器希望获取什么类型数据
  * Content-Type：指明请求体数据类型，text/html、text/plain、application/json。
* 请求体
  前端需要交个服务器端数据，可以是Form表单形式题及其哦，也可以是json形式提交

## 2）请求方法

* GET：`在服务器端获取资源`
* POST：`在服务器端新增资源`
* PUT：`在服务器端更新资源`
* DELETE：`在服务器端删除资源`

<font color=red>**四种请求对应了对服务器端资源的增删改查四种操作，这只是一套指导规范，具体以入职以后的公司风格为准。**</font>

## 3）HttpRequest请求对象

* 说明
  视图函数的第一个参数是HttpRequest请求对象，服务器端收到请求后，会根据请求数据报文创建请求对象。

* 请求对下属性

  |  属性  |            作用            |  数据类型   |
  | :----: | :------------------------: | :---------: |
  | method |          请求方法          |   字符串    |
  |  GET   |         查询字符串         |  QueryDict  |
  |  POST  | 请求体数据：以表单形式提交 |  QueryDict  |
  |  body  | 请求体数据：以json形式提交 | 字节串：b"" |
  |  META  |         请求头数据         |    字典     |

  注意：<font color=green>**request.META:Django会把所有请求头都大写，并加上HTTP前缀，比如：HTTP_ACCEPT、HTTP_USER_AGENT等。也可以获取客户端的IP地址：REMOTE_ADDR**</font>

### 4）关于响应

#### 4.1 响应对象HttpResponse

- 视图函数中
  return HttpResponse(content = '响应体', content_type = '类型', status = 自顶一个状态码)

  content_type示例:

  * html格式：`text/html;charse=utf-8`
  * 文本格式：`text/plain;charset=utf-8`
  * json格式：`application/json;charset=utf-8`

#### 4.2 目前遇到的响应状态码

* 404:Page Not find，检查路由和浏览器URL地址是否匹配
* 403:403 Forbidden,csrf校验未通过，关闭csrf中间件。
      settings.py中MIDDLEWARE：csrf中间件注释掉。
* 500：服务器错误，检查视图函数。

# DJANGO-DAY02

## 一、设计模式

<font color=red>**所有设计模式的最终作用都是为了降低模块间的耦合度**</font>

1）MVC

* M:模型层model，封装数据库层
* V:视图层View，展示数据
* C:控制器层Controller，<font color=red>**核心**</font>

2）MTV

* M:模型层model，封装数据库层
* T:模板层Templates，展示数据
* V:视图层View，<font color=red>**核心**</font>，处理用户请求，获取数据、返回结果

## 二、模板层Template

### 1）定义

可以根据视图中传递的字典数据动态生成响应的HTML网页，并呈现给用户。

### 2）使用流程

* 项目目录或者应用目录下创建模板目录：templates

* settings.py中配置模板配置项（**TEMPLATES**）

  1. BACKEND：指定模板引擎（<font color=red>**默认**</font>）
  2. DIRS：指定模板的搜索目录（<font color=red>**[BASE_DIR/"templates"]**</font>）
  3. APP_DIRS：是否在应用的templates目录中搜索模板文件（<font color=red>**默认**</font>）
  4. OPTIONS：有关模板的选项（<font color=red>**默认**</font>）

* 创建模板html文件

* 视图函数中reder()方法加载模板
  ```python
  from django.shortcuts import render
  def xxx_view(requset):
      return render(request, "html文件名", 字典数据)
  ```

#### 3）模板语言

#### 3.1模板传参

`return render(request, "xxx.html", 字典数据)`

#### 3.2模板变量

* 语法格式：`{{  }}`
* 分类
  1. 普通变量：{{ 变量名 }}
  2. 列表中指定索引元素：{{ 变量名.index }}
  3. 获取字典中指定的key：{{ 变量名.key }}
  4. 获取指定函数返回值：{{ 函数名 }}
  5. 获取对象数据：{{ 对象名.方法 }}

#### 3.3模板标签

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
  ~~~python
  {% for 变量名 in 可迭代对象 %}
  ...
  {% empty %}
  ...
  {% endfor %}
  ~~~

* 加载静态文件
  ~~~python
  {% load static %}
  <img src="{% static 'bf.png' %}">
  ~~~

# 三、静态文件配置

* settings.py中配置请求路由和寻址路径
  ~~~python
  STATIC_URL = "/static/"
  STATICFILES_DIRS = (BASE_DIR / "static",)
  ~~~

* 羡慕目录下穿件static目录，并存放图片

* 浏览器测试
  http://127.0.0.1:8000/static/bf.png

# 四、应用APP

## 1）定义

应用是在django项目中独立的一个业务模块，每个应用包含自己的路由、视图、模型、模板。

## 2）使用流程

1. 创建应用：python3 manage.py startapp 应用名

2. 注册应用：settings.py中INSTALLED_APPS：[应用名,]

3. 匹配主路由：<font color=red>**注意：iclude()方法**</font>
   ~~~python
   from djangp.urls import path, include
   path("主路由", include("应用名.urls"))
   ~~~

4. 分配分布式路由：<font color=red>**单独创建urls.py**</font>

5. 完成视图

<font color=green>**注意：查找模板文件时，有限查找外层的templates目录下的模板文件，如果没有则安之settings.py中INSTALLED APPS中应用注册的顺序一次逐个查找**</font>

# DJANGO-DAY03

## 一、DJANGO配置MySQL

* 创建数据库
  `create database 库名`

* 配置数据库<font color=red>**settings.py**</font>
  <font color=red>**指定数据库引擎、主机地址、用户名、密码、端口号、具体库**</font>
  示例

  ~~~python
  DATABASES = {
      "default" : {
          "ENGINE" : "django.db.backends.mysql",
          "HSOT" : "MySQL数据库服务器的IP地址",
          "USER" : "用户名",
          "PASSWORD" : "密码"
          "NAME" : "库名",
          "PORT" : "3306"
      }
  }
  ~~~

  * 常见的数据库引擎
    django.db.backends.mysql
    django.db.backends.sqlite3
    django.db.backends.oracle

## 二、ORM框架

### 1）定义

ORM，对象关系映射，避免使用SQL操作数据库，而是使用类和对象对数据库进行操作。

### 2）ORM特点

#### 2.1 优点

* 只需要面向对象编程，无需面向数据库编程。
* 实现数据模型和数据库的解耦，不用关心公司使用的数据（**MySQL、Oracle等**）的细节，通过简单的配置就能实现更换项目数据库，无需修改大量代码。

#### 2.2 缺点

* 需要将对应的操作语句转为SQL命令再执行，映射的过程中会有一定程度的性能丢失。
* 对于复杂的业务使用成本较高。

### 3）ORM对应关系

|    ORM     |   数据表   |
| :--------: | :--------: |
|   一个类   |   一张表   |
| 一个类属性 | 一个表字段 |
|  一个对象  | 一条表记录 |

得到对象：

1. user = UserProfile.objects.create(username="xxx", .\..)
2. user_query = UserProfile.objects.filter(username="xxx")
   user = user_query[0]

### 4）ORM使用流程

1. 创建库

2. 配置数据库

3. 创建应用
   `python3 manage.py startapp 应用名`

4. 创建模型类
   ~~~python
   from django.db import models
   
   
   class Xxx(models.Model):
       username = models.CharField(...)
   ~~~

5. 迁移同步
   python3 manage.py makemigrations
   python3 manage.py migrate

### 5）迁移文件混乱的处理方法

1. 删除库：`drop database 库名;`
2. 建库：`create database 库名`
3. 删除迁移文件：`删除所有应用下的migrations中的000_xxx...的文件`
4. 迁移同步
   python3 manage.py makemigrations
   python3 manage.py migrate

### 6）字段类型

* 类型

  |    models.py    |     mysql数据库     |     示例     |
  | :-------------: | :-----------------: | :----------: |
  | BooleanField()  |     tinyint(1)      | 是否会员.\.. |
  |   CharField()   |     varchar(20)     |              |
  | DateTimeField() | datetime、timestamp | 注册时间.\.. |
  | DecimalField()  |    decimal(M,D)     |   单价.\..   |
  |  EmailField()   |      varchar()      |     邮箱     |
  | IntegerField()  |         int         |   销量.\..   |

* 字段类型需要注意

  1. 字符类型必须指定宽度：**CharField(max_length=宽度)**
  2. 浮点型必须指定总位数和小数位位数：**DecimalField(max_digits=10, decimal_places=2)**
  3. 日期时间类型设置自动创建或者更新时间：
     **auto_now_add=True:插入数据时自动创建时间**
     **auto_now=True:增删改时会自动更新时间**
  4. 存储邮箱：**EmailField()**

* 字段选项注意

  1. 主键：primary_key
     <font color=red>模型类未指定主键，则Django会自动创建id自Duang并设置为主键；如果模型类指定主键，则Django不会创建id字段</font>
  2. 索引：db_index
     <font color=red>在经常用来查询的字段上建立索引，也是数据库优化的一种方式</font>
  3. 唯一：unique
     字段的值不允许重复

### 7）内部类Meta

使用Meta内部类，可以对模型类赋予属性，比如指定表名、指定admin后台数据格式显示等等。

# DJANGO-Day04

## 一、ORM增删改查

<font color=blue>**统统使用管理器对象objects，每个继承models.Model的模型类都会同样继承objects**</font>

### 1）增-create

* 方法：模型类.objects.create(属性=值, 属性=值, .\..)

### 2）查-filter

* 方法：模型类.objects.filter(条件1, 条件2, .\..)
