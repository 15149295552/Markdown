# web概述

web就是互联网上的一种应用程序 - 网页

## 典型的程序： [python web.md](python web.md)

​	1.C/S
​		C:Client(客户端)
​		S:Server(服务端)
​	2.B/S
​		B:Brower(浏览器)
​		S:Server(服务端)

## web组成 & 运行流程

由服务器，浏览器和通信协议组成
服务器：处理用户的请求和响应
浏览器：代替用户向服务器发送请求(USER Agent)
通信协议：规范了数据是如何打包和传递的
	http:Hyper Text Transfer Protocal
		 超级   文本   传输	 协议

前端：HTML + CSS + JS
后端：Python + Django + MySQL

# HTML

## 概念

Hyper Text Markup Language
 超级  文本  标记    语言

超文本：具备特殊功能的文本
	普通文本a：普通字符a
	超文本a：表示超链接的功能
	普通文本b：普通字符b
	超文本b：表示文字加粗的功能

标记：超文本的组成形式
	普通文本：a
	超文本：<a></a>
	超文本：<b></b>

## HTML在计算机在的表项

所有网页(HTML)文件在计算机都是以.html或者.htm作为结尾的文件

## 语法

### 标记语法

标记：在网页中，用于表示功能的符号称为"标记/标签/元素"

语法：所有的标记在使用时，必须用<>括起来

分为单标记和双标记

  双标记：显示的开始和显示的结束

  <标记>内容</标记>
  ```html
  <a> . . .</a>
  ```


  注意：双标记，有开始，必须有结束，否则会报错

  单标记：只有一个标记，技能表示开始，也能表示结束
  <标记>或者<标记/>

```html
<br> 或者 <br/> ：换行
<hr> 或者 <hr/> : 水平线
<img> 或者 <img/>: 一幅图像
```

嵌套:在一对标记中，再嵌套其他的标记，相当于是功能的嵌套
加粗的超链接

```html
<a><b>百度</b></a>
<b><a>百度</a></b>
```

练习：
```html
<!--文档类型声明 -->
<!DOCTYPE html>
<!--网页根标记-->
<html lang="en">
    <!--网页头部-->
    <head>
        <!--网页编码-->
    	<meta charset="UTF-8">
    	<!--网页标题-->
        <title>bf</title>
    </head>
    <!--网页主体-->
    <body>
        bf
    </body>
</html>
```

```html
<html>--父元素/父标记
	<head></head>--子元素/子标记
	<body></body>--子元素/子标记
</html>
```

起始格式
```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>bf</title>
    </head>
    <body>
        
    </body>
</html>
```



属性和值：在标记中，用来修饰标记显示效果的内容

语法：
	1、属性必须声明在开始标记中
		<标记属性的声明位置处></标记>
	2、属性和标记名称之间要用空格隔开
		k标记属性名称></标记>
	3、属性和值之间是用那个=连接，属性值要用“”或‘’引起来
		亚标记属性名称='值'>
	4、一个元素允许有多个属性，并且排名不分先后，多属性之间使用空格隔开即可
		<标记 属性1='值1'属性2='值2'>

练习：

```html
<!DOCTYPE html>
<html lang="en">
    <head>
    	<meta charset="UTF-8">
        <title>bf</title>
    </head>
    <body>
        <p align="center" id="p1">
            this is p
        </p>
    </body>
</html>
```

HTML中的注释
<!-- -->
注意：
注释不能出现在<>中
~~<p<!---->></p>~~
注释不能嵌套

### 文档结构

文档声明

出现在网页的最顶端的第一个标记
作用：告诉浏览器使用HTML的版本

```html
<!DOCTYPE html>或者<!doctype html> - 使用HTML5版本
```

HTML页面
在文档类型声明之下，使用一对html根标记组成
<html></html>
在html中包含两对子元素

```html
<head></head> - 表示网页的头部
<body></body> - 表示网页的主题
```

练习：
01_text.html

```html
<!doctype html>
<html>
    <head>
        <title></title>
        <meta charset="utf-8">
    </head>
    <body>
        <p>&nbsp;
            这是一段文本
        </p>
    </body>
</html>
```

标记
特殊字符

```html
&nbsp;  表示一个空格
&lt;	表示 <
&gt		表示 >
&copy;  表示 ©
&yen;	表示 ￥
```

样式标记
作用：修改文本在网页中的表现形式

```html
<i></i> :斜体显示文本
<u></u> :下划线显示文本
<s></s> :删除线的方式显示文本
<b></b> :加粗显示文本
<sup></sup>:以上标的形式显示文本
<sub></sub>:以下标的形式显示文本
```

标题标记
作用：以不同的文章大小以及加粗的方式显示文本
语法：

```html
<h#></h#>
#:1-6
h1:一级标题
h2:二级标题
...
h6:六级标题
```

```html
<!doctype html>
<html>
    <head>
        <title></title>
        <meta charset="utf-8">
    </head>
    <body>
        <p>&nbsp;
            这是一段文本
        </p>
        <h1 align="left">
           静夜思
        </h1>
        <h3 align="center">
            李白
        </h3>
        <h2 align="right">
            床前明月光
        </h2>
        <h2>
            疑是地上霜
        </h2>
		<h2>
            举头望明月
        </h2>
		<h2>
            低头思故乡
        </h2>
    </body>
</html>
```

段落元素
作用：突显一段文本，每段文本独占一行/块，并且每个段落都会具备一小段的垂直空白距离
语法：

```html
<p></p>
属性：align
取值：left/center/right
```

练习：
```html
<p align="center">
    天长地久有时尽
	<br>此恨绵绵无绝期
</p>
```

换行元素
```html
<br>或</br>
```

分区元素
快分区

```html
<div>
    
</div>
```

特点：独占一行/块
作用：在网页中做布局（配合css)

行内分区元素

```html 
<span></span>
```

特点：允许在一行内显示多个span,并能够与其他的文本在一行内显示
作用：设置同一行文本的不同样式（配合css)

```html
<div>
    这是快分区元素
</div>
<span>这是行内分区元素</span>
```

行内元素与块级元素

块元素：只要能在网页中能独占一行的元素就都是块元素
div,p,h1…h6
所以元素都具备align属性，取值为left/center/right

行内元素：多个元素能够在一行内显示，就是行内元素
span,i,b,s,u,sup,sub

列表
	类型
	有序 - <ol></ol>
	无序 - <ul></ul>

列表项 - <li></li>
```html
<ol>
    <li>内容</li>
</ol>
<ul>
    <li>内容</li>
</ul>
```

练习：
```html
<!doctype html>
<html>
    <head>
        <title></title>
        <meta charset="utf-8">
    </head>
    <body>
        <ol>
            <li>西游记</li>
    		<l1>三国演义</Li>
			<1i>红楼梦</Li>
			<1i>水浒传</Li>
		</ol>
		<ul>
            <li>金毛狮王</li>
			<li>白眉鹰王</li>
   			<li>青翼蝠王</li>
			<li>紫衫龙王</li>
		</ul>
    </body>
</html>
```

列表属性

```html
<ol>
    
</ol>
```

属性：type
取值：
1、1:按数字方式排列，默认值
2、A:按大写英文字符方式排列
3、a:按小写英文字符方式排列
4、I:按大写罗马数字方式排列
5、1:按小写罗马数字方式排列

```html
<ul>
    
</ul>
```

属性：type
取值：
1、disc:实心圆点，默认值
2、circle:空心圆点
3、square:实心方块
4、none:不显示任何标识

列表的嵌套：在一个列表项中，又出现一个列表
```html
<ul>
    <li><ol>
        <li></li>
        </ol>
    </li>
</ul>
```

图像和超链接

URL
Uniform Resource Locator
  统一	资源	定位符
用于标识网络中资源的位置，俗称路径

URL分类
绝对路径：访问互联网上的资源时，使用绝对路径
相对路径：从当前文件所在的位置处去查找资源文件所经过的路径
	1.同目录下
	2.在子目录下
	3.在父目录刊
	./ 表示当前路径 ../ 表示上一级路径

注意：
UrL中不能出现中文
UrL是严格分区大小写

图像
标记：<img>
属性：
src：指定要显示的图片的URT
width：宽度，以px为单位的数值（允许省略px)
height：高度，以px为单位的数值（允许省略px)

```html
<img src="img/1.jpg"width="600",height="400">
```

练习：
```html
<!doctype html>
<html long="en">
    <head>
        <meta charset="utf-8">
        <title>Title</title>      
    </head>
    <body>
        <h1 align="center">HTML5 &lt;Day02&gt;</h1>
		<hr>
        <h2>
            1 HTML 文档片段
        </h2>
        <h3>
            1.1 问题
        </h3>
        <p>
            创建如图-1所示的html页面
        </p>
        <div>
            <img src="img/page,JPG">
        </div>
        <h4>
            图-1
        </h4>
		<h3>
            1.2方案
        </h3>
		<p>
            使用HTML完成效果
        </p>
        <div align="center">
            <a href="https://www.baidu.com/"target="_blank">
			<img src="img/page_struct.JPG">
			</a>
			<h4>
                图-2
            </h4>
		</div>
    </body>
</html>
```

### 表格

语法：
```html
<table>
    <tr>表行</tr>
    <td>单元格(列)</td>
</table>
```

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>bf</title>
    </head>
    <body>
        <table border="1" width="400" height="300" align="center">
            <tr align="center">
                <td>姓名</td>
                <td>语文</td>
                <td>数学</td>
                <td>英语</td>
            </tr>
            <tr align="center">
                <td>老王</td>
                <td>85</td>
                <td>72</td>
                <td>63</td>
            </tr>
            <tr align="center">
                <td>张三</td>
                <td>76</td>
                <td>78</td>
                <td>79</td>
            </tr>
            <tr align="center">
                <td>李四</td>
                <td>98</td>
                <td>45</td>
                <td>56</td>
            </tr>
        </table>
    </body>
</html>
```

属性
table属性
border：指定表格边框的宽度
width：指定表格的宽度
height：指定表格的高度
align：指定表格在其父元素中的水平对齐方式
	取值：left/center/right
cellpadding：单元格内边距，内容与单元格边框之间的距离
cellspacing：单元格外边距，单元格与单元格之间的距离

tr属性
align：控制当前行的内容的水平对齐方式
	取值：left/center/right
valign：控制当前行的内容垂直对齐方式
	取值：top/middle/bottom
bgcolor：指定当前行的背景颜色
	取值：颜色的英文单词
td属性：
width
height
align
valign
bgcolor
colspan:跨列/合并列
rowspan:跨行/合并行

行分组
表头行分组：<thread></thread>
表主题分组：<tbody></tbody>

### 表单

作用：用于接收用户的数据并提交给服务器

要素：
form元素 - 表单：收集用户的信息
表单控件：提供能够与用户进行数据交互的可是黄组件

form
语法：

```html
<form>
    
</form>
```

属性：
action：指定要提交给服务器程序的地址
method：提交方式
	get(默认方式)：表示向服务器要数据时使用
		特点：会将提交的数据显示在地址栏上；安全性较低
	post：表示要将数据提交给服务器进行处理时使用
		特点：隐式提交，看不到提交的数据；安全性较高

表单控件
文本框：<input type="text">
密码框：<input type="password">
属性：
name：定义控件的名称(提交给服务器使用，如果没有name的话则无法提交)
value:值
maxlength:限制输入的最大字符数
readonly:只读
placeholder:占位符，用户未输入任何数据时所显示的内容

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>bf</title>
    </head>
    <body>
        <form action="" method="get">
            <p>
                用户名称:<input type="text" name="uname" placeholder="请输入用户名">
            </p>
            <p>
                用户密码:<input type="password" name="upwd" placeholder="请输入密码">
            </p>
        </form>
    </body>
</html>
```

按钮
提交按钮：<input type="submit">
将表单的数据提交给服务器

重置按钮：<input type="reset">
将表单的内容恢复到初始化状态

普通按钮：<input type="button">
允许通过js自定义按钮
属性：
value:指定按钮上的文本

```html
<button>
    也可以定义为按钮
</button>
```

属性：type
	取值submit/reset/button

单选框和复选框
单选按钮：<input type="radio">
复选按钮：<input type="checkbox">
属性：
name：除了定义名称，还可以分组
		一组中的单选按钮或复选按钮,name属性必须一致

value：值，提前声明好

checked：设置预选中

文件选择框：上传文件时使用
语法：<input type="file">
属性：name,控件名称

多行文本域
标记：

```html
<textarea></textarea>
```

属性：
name：控件名称
cols：指定文本域默认显示的列数
rows：指定文本域默认显示的行数

下拉选择框
语法：

```html
<select>
    <option>显示</option>
    <option>1</option>
</select>
```

属性：
name
value

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>bf</title>
    </head>
    <body>
        <form action="" method="get">
            <p>
                用户名称:<input type="text" name="uname" placeholder="请输入用户名">
            </p>
            <p>
                用户密码:<input type="password" name="upwd" placeholder="请输入密码">
            </p>
            <p>
                <input type="checkbox" name="hobby" value="1">1
                <input type="checkbox" name="hobby" value="2">2
                <input type="checkbox" name="hobby" value="3">3
                <input type="checkbox" name="hobby" value="4">4
            </p>
            <p>
                上传头像：
                <input type="file" name="uimg">
            </p>
            <p>
                自我介绍：
                <textarea name="intro" id="" cols="30" rows="10"></textarea>
            </p>
            <p>
                <select name="city">
                    <option value="0">北京</option>
                    <option value="1">上海</option>
                </select>
            </p>
            <p>
                <input type="submit">
                <input type="button">
                <input type="reset">
            </p>
        </form>
    </body>
</html>
```

# CSS

CSS - 样式表
HTML：搭建网页结构
CSS：修饰和美化网页

内联方式
将CSS的内容定义在单独的HTML标签中
语法：

```html
<ANY style="样式声明"></ANY>
```

样式声明：由样式属性和样式值来组成
属性和值之间使用：连接
<ANY style="属性：值">
在style中允许有多个样式声明，多个样式声明之间使用;隔开
<ANY style="属性:值;属性:值">

文字大小：font-size
	取值：以px为单位的数字
	ex:设置某元素的文字大小为18px
	<ANY style="font-size:18px;">

文本颜色：color
	取值：颜色英语单词
	ex:设置某元素的文字大小为18px,文本颜色为红色
	<ANY style="font-size:18px;color:red;">

背景颜色：background-color
	取值：表示颜色的英文单词
	<ANY style="background-color:yeLlow;">

练习：
```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>bf</title>
    </head>
    <body>
        <div style="font-size:24px;color:red;background-color:yellow">
            这是一个测试的div标记
        </div>
    </body>
</html>
```

内部样式表：让定义好的样式能够使用在当前页面中的多个元素上
语法：

```html
<head>
    <style>
        样式规则
        样式规则
        ...
    </style>
</head>
```

样式规则
语法：由选择器和样式声明组成
选择器：规范页面中哪些元素能够使用声明好的样式
选择器{
	属性1:值1
	属性2:值2
}

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>bf</title>
        <style>
            div{
                font-size:24px;
                color:red;
                background-color:yellow;
            }
        </style>
    </head>
    <body>
        <div>
            这是一个测试的div标记
        </div>
        <div>
            这是一个又测试的div标记
        </div>
    </body>
</html>
```

外部样式表：将声明好的样式应用在多个网页中
		   将样式规则声明在独立的css文件中(***.css)
			在使用的网页中对css文件进行引用

使用步骤

1. 创建css文件，并编写样式规则

2. 引出css文件
   ```html
   <head>
       <link rel="stylesheet" href="style.css">
   </head>
   ```

```css
p{
    font-size:18px;
    color:red;
    background-color:green;
}
div{
    ...
}
```

内联方式，最高；内部样式表和外部样式同样高(就近原则：后定义者优先)

css选择器：规范了页面中哪些元素能够使用声明好的的样式
			目的为了匹配页面的元素
选择器详解
元素选择器
特点：由标记名称作为选择器，主要匹配页面中指定标记所对应的所有元素
语法：
标记{
样式声明；
}

类选择器
特点：允许被任意一个元素所引用的选择器
语法：
.类名{
	…
}
类名：字母、数字、_ 、-组成；数组不能开头
引用：<ANY class="类名">

特殊用法
分类选择器的定义方式
允许将元素选择器和类选择器结合到一起使用，为了实现对某种元素不同样式的细分控制
语法：
元素选择器.类选择器{
…
}
div.font{
	匹配页面中所有class为font的div元素
}

多类选择器的引用方式
  允许让一个元素同时引用多个类选择器，多个类选择器之间使用空格隔开
  <ANY class="c1 c2 c3">

练习：
```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>bf</title>
    </head>
    <body>
       <div>
           这是div中的内容
        </div>
        <h3>
            h3的内容
        </h3>
        <p>
            p的内容
        </p>
        <span>
        	span的内容
        </span>
    </body>
</html>
```

id选择器
id作用：在HTML中，每个元素都允许设置一个1d属性，主要用于表示该元素在网页中独一无二的标识<div id="main"></div>
语法：

```html
# id选择器值
#main{

}
```

群组选择器
作用：定义多个选择器们的共有的样式
	  定义方式是一个以，隔开的选择器列表
语法：
选择器1，选择器2，选择器3，…{

}

# Django框架

web与服务器

web：表示用户可以浏览的网页内容(HTML，CSS，JS)

服务器：能够给用户提供的机器
硬件范畴：一台机器
软件范畴：一个能够接收用户请求并给出相应的程序
作用：存储web所需要的信息(HTML，图片，音频)
	  能够处理用户的请求request，并给出响应response
	  执行服务器端的程序：查找数据库
Django框架：使用Python语音开发服务器端处理程序

框架：框架是一个为了解决开放性问题而存在的一种结构
	框架本身提供一些最基本的功能，我们只需要在基础功能上搭建属于自己的操作即可

Python中的框架
Django：重量级的web框架
Tornado：异步框架
Flask：轻量级框架

Django框架：是一个开源框架，采用Python语言开发
模式 - MTV
M:Models层
	模型层，负责数据库的建模以及CRUD的操作
T:Templatets层
	模板层，用于处理显示用户显示的内容，录：html
V:Views层
	视图层，处理与用户交互的部分内容，从模型中获取数据，再将数据发送给模板，显示给用户

使用
创建Django
django-admin startproject 项目名

启动服务 用于访问
在创建好的项目中，找到manage.py文件
通过manage.py启动项目(服务)
python3 manage.py runserver

启动服务之后，在浏览器中，通过网址访问
http://127.0.0.1:8000
http://localhost:8000

项目结构
manage.py
负责执行django中的各项操作的文件
如：
	启动服务
	创建应用
	数据库的同步

2、主文件夹(名称与项目名称相同)
__ init __.py
项目初始化文件，每当服务器启动的时候，都会自动执行

urls.py
项目的基础url(路由)配置文件

wsgi.py
应用服务器配置文件，暂时不用

settings.py
项目的主设置文件:应用，模板，数据库，语言，时区，... ...
BASE_DIR : 获取当前项目的绝对路径
DEBUG : 调试模式
	开发过程：推荐使用 True
	上线运行：必须改为 False
ALLOWED_HOSTS
	设置允许访问本项目的地址列表。
	如果不设置的话，只有本机(localhost/127.0.0.1)能访问。
	推荐写 '*',任何表示该机器的地址都可以访问当前项目
INSTALLED_APPS
	指定已安装的应用，如果有自定义的应用的话，需要在此注册
MIDDLEWARE:注册中间件
ROOT_URLCONF:指定项目的基础路由配置文件
TEMPLATES:指定模板的信息
DATABASES:指定数据库的信息
LANGUAGE_CODE:指定语言，允许修改为zh-Hans
TIME_ZONE:指定时区，建议改为 Asia/Shanghai

URL的使用
urls.py
默认在主文件夹中，主路由位置文件，包含所有的地址映射
每一个请求到达之后，都会由urls.py中的urlpatterns列表中的url()进行匹配
url()函数匹配上之后，可能将请求转交给其他的Views(视图)或其他的urls.py去处理

python3 manage.py migrate

创建超级管理员
python3 manage.py createsuperuser
1.用户名
2.邮箱
3.密码
4.确认密码

url函数的语法
url(regex,views,kwargs=None,name=None)
1.regex
正则表达式模板，匹配请求的url
2.views
URL处理的视图函数
3.kwargs
字典，用来向views传参的，没有参数可以省略
4.name
字符串，给url()起别名，主要在模板中使用

Django应用
应用就是网站中的一个独立的模块程序
在Django中，主目录一般不处理用户的请求，主要做的是项目的初始
化以及请求的分发

创建应用
命令
python3 manage.py startapp 应用名称

在settings.py中进行注册
在INSTALLED_APPS中追加应用名称
INSTALLED_APPS =[
'django.contrib.admin',
'应用名称'
]

结构组成
migrations目录
存放数据库中间文件的目录（日志文件）

\__init__.py
应用的初始化文件

admin.py
应用的后台管理配置文件

apps.py
应用的属性配置文件，不需改动

modets.py
Models文件

tests.py
测试模块

views.py
定义视图的文件

Django模板(Template)
模板是要动态给用户呈现的网页内容
可以由Views(视图)呈现给用户
其实就是网页 - 前后端结合的一个网页

模板设置
在setting.py中TEMPLATES 变量
TEMPLATES = [
	{
		'BACKEND':'... ...',
		'DIRS':[... ..],
		... ..
	},
]

BACKEND:指定模板的搜索引擎，不用动
DIRS:指定模板所存放的目录们
DIRS ['index.temp','music.temp']

模板加载方式
使用render直接加载并返回模板
from django.shortcuts import render
def xxx_views(request):
return render(request,'模板名称',{})

标签：允许将服务器端的一些功能嵌入到模板中
语法：{% 标签内容 %}
常用标签：for
	{% for 变量 in 列表|元组|字典 %}
	{% endfor %}

​		if
​	{% if 条件 %}
​		\<p></p>
​	{% endif %}

​		if ..else...
​	{% if 条件 %}
​		满足条件执行的内容
​	{% else %}
​		不满足条件执行的内容
​	{% endif %}

​		if ...elif ..elif ...else
​	{% if 条件 %}
​		满足条件1运行的内容
​	{% elif 条件2 %}
​		满足条件2运行的内容
​	{% else %}
​		以上条件都不满足时运行的内容
​	{% endif %}

静态文件：模板中所需要用到的到的css,js,image等一些资源文件都是静态文件

Django中静态文件的处理
需要在settings,py中设置静态文件的访问路径和存储路径
STATIC_URL
	指定静态文件的访问路径
	STATIC_URL='/static/'
作用：
当访问路径是Localhost:8000/static/***
一律都去静态文件存储路径中搜索静态文件

存储路径
STATICFILES_DIRS
指定静态文件存储路径
STATICFILES_DIRS=(BASE_DIR,static')
'static':当前项目存放静态文件的目录名
在项目的sta中c目中以及所有应用的static目录中存放就是静态文件

访问静态文件
直接使用L0 calhost:8000/static/***
\<img src="/static/img/timg.jpeg">

使用{% static %}访问静态资源
在模板的最顶层增加
{% load static %}
在使用静态资源时
{% static %}表示的就是静态资源访问路径
\<img src="{% static 'img/timg.jpeg' %}">

练习：
创建项目
django-admin startproject fruitday
python3 manage.py startapp login
创建文件
login/static/css/login.css
login/static/img/huiyuan.jpg
login/templates/login.html

代码：
fruitday/urls.py

```py
from django.conf.urls import url, include
from django.contrib import admin

urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'login/', include('login.urls')),
]
```

login/urls.py
```py
from django.conf.urls import url
from .views import login_views
urlpatterns=[
    url(r'^$',login_views)
]
```

login/views.py
```py
from django.shortcuts import render
# Create your views here.

def login_views(request):
    return render(request,'login.html')
```

login.html
```html
{% load static %}
<!doctype html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>天天果园-会员登录</title>
     <link rel="stylesheet" href="{% static 'css/login.css' %}">
    </head>
    <body>
        <div id="container">
            <!-- 上 ：会员登录 -->
			<h2>会员登录</h2>
			<!-- 下左：登录图像 -->
			<p>
				<img src="{% static 'img/huiyuan.jpg' %}">
				<a href="#">会员注册&gt;</a>
			</p>
			<!-- 下右：登录表单 -->
            <div id="login">
                <form action="login" method="post">
                    <!-- 第一行：手机号 -->
                    <div class="form-line">
                        <p>手机号</p>
                        <div>
                            <input type="text" name="uphone" class="form-control">
                        </div>
                    </div>
                    <!-- 第二行：密码 -->
                    <div class="form-line">
                        <p>密码</p>
                        <div>
							<input type="password" name="upwd" class="form-control" placeholder="请输入密码">
						</div>
					</div>
                    <!-- 第三行：记住密码，忘记密码-->
                    <div class="form-line">
						<p></p>
                        <div>
							<!-- 右浮动 -->
							<p>
                                <a href="#">忘记密码</a>
								<a href="#">快捷登录</a>
							</p>
							<!-- 不浮动 -->
							<input type="checkbox" name="isSaved" class="isSaved" checked>记住密码
						</div>
                    </div>
					<!-- 第四行：登录按钮-->
					<div class="form-line">
						<p></p>
						<div>
							<!-- 右：超链接  -->
							<a href="#" class="goReg">注册会员</a>
							<!-- 左：登录按钮 -->
							<input type="submit" value="登录" class="btnLogin">
						</div>
					</div>
				</form>
			</div>
		</div>
 	</body>
</html>
```

login.css
```css
p,h2{
	margin:0;
}

#container{
	width:990px;
	margin:0 auto;
}

#container>h2{
	color:#999;
	font-weight:normal;
	border-bottom:1px solid #ccc;
	padding-bottom:20px;
	margin-bottom:20px;
}

#container>p{
	float:left;
}
#login{
	float:right;
}

#login .form-line>p{
	float:left;
	font-size:16px;
	color:#999;
	width:64px;/*每个文字16px的宽，按照4个文字的宽度给*/
	text-align:right;
	margin-right:20px;
	/*行高：为了与文本框垂直居中对齐*/
	line-height:40px;
	/*高度：为了把自己给撑起来*/
	height:40px;
}

#login .form-line>div{
	float:left;
	width:300px;
}

#login .form-control{
	/*宽度，高度，边框，box-sizing，取消轮廓，左右内边距*/
	width:300px;
	height:40px;
	border:1px solid #ccc;
	box-sizing:border-box;
	outline:none;
	padding:0 15px;
}
#login .form-control:focus{
	/*边框颜色，边框阴影*/
	border-color:#64a131;
	box-shadow:0 0 5px #64a131;
}

#login .form-line{
	margin-bottom:25px;
	/*overflow：hidden : 目的是将 .form-line 的高度撑起来*/
	overflow:hidden;
}

#login .form-line>div>p{
	float:right;
}
#login .form-line>div>p a{
	color:#999;
	text-decoration:underline;
}
#login .isSaved{
	width:18px;
	height:18px;
	vertical-align:middle;
}

#login .goReg{
	/*145*38*/
	/*右浮动，宽度，高度，边框(#64a131)，边框倒角，box-sizing，文本水平居中对齐，垂直居中对齐(行高)，取消下划线，文本颜色(#7BAE55)，文字大小，背景颜色(#F5FFED)*/
	float:right;
	width:145px;
	height:38px;
	border:1px solid #64A131;
	border-radius:5px;
	box-sizing:border-box;
	text-align:center;
	line-height:38px;
	text-decoration:none;
	color:#7BAE55;
	font-size:18px;
	background-color:#f5ffed;
}
#login .btnLogin{
	/*宽度，高度，取消边框，边框倒角，背景颜色，文本颜色，文字大小*/
	width:145px;
	height:38px;
	border:none;
	border-radius:5px;
	background-color:#64a131;
	color:#fff;
	font-size:18px;
}

/*会员注册超链接*/
#container>p{
	/*相对定位：配合着里面的超链接做绝对定位*/
	position:relative;
}
#container>p>a{
	/*绝对定位，要在 p 元素中 实现位置的偏移*/
	position:absolute;
	bottom:25px;
	left:173px;
	/*宽度，高度，边框，box-sizing，倒角，文字大小，文字颜色，水平居中，垂直居中(行高)*/
	width:154px;
	height:48px;
	border:1px solid #64A131;
	box-sizing:border-box;
	border-radius:5px;
	font-size:18px;
	color:#64A131;
	text-align:center;
	line-height:48px;
}
```

模型 - Models
模型是根据数据库中表结构来创建出来的class
每一张表到编程语言中就是一个class
表中的每一列，到编程语言中就是class中的一个属性
在模型中完成对数据的CRUD操作
C:Create
R:Retrieve
U:Update
D:Delete

创建和使用模型 - ORM创建
ORM:Object Relational Mapping(对象关系映射)
三大特征
数据表到类(class)的映射
	将数据表自动生成一个class类
	将class类自动生成数据库中的一张表
数据类型的映射
	允许将表中字段的数据类型自动映射成编程语言中对应的数据类型
	也允许将编程语言的数据类型自动映射成表中的字段的数据类型
关系映射
	数据库中表的关联关系：一对一，一对多，多对多

ORM的优点
提高了开发效率，能够自动完成表到对象的映射，可以省略庞大的数据访问层
不用SQL编码，也能够完成对数据的CRUD操作

创建和配置数据库
创建数据库（支持中文）
create database数据库default charset utf8 collate utf8_general_ci;

连接MySQL的配置：
ENGINE:引擎
	django.db.backends.mysql
NAME:要连接到的数据库名称
USER:用户名称，通常为root
PASSWORD:密码，123456
HOST:要连接的主机，本机的话localhost或127.0.0.1
PORT:端口，MYSQL的是3306

```PY
DATABASES = {
    'default':{
        'ENGINE':'django.db.backends.mysql',
        'NAME':'fruitdaydb',
        'USER':'root',
        'PASSWORD':'',
        'HOST':'localhost',
        'PORT':'3306'
    }
}
```

\__init__.py
import pymysql
pymysql.install_as_MySQLdb()

编写Models
Models中的每个class都称为模型类(Model类)或实体类(Entry)
	实体：表示的就是数据表中的一条记录
	实体完整性：约束表中的记录不完全重复
Models中的每个类都必须继承自models.Model

同步数据库
python3 manage.py makemigrations
python3 manage.py migrate

5Dang0中提供的数据子段和子段选项
语法：
属性=models.数据字段（字段选项）
数据字段(Field Type)
1、BooleanField(
2、CharField()
3、DateField)
4、DateTimeField()
5、DecimalField()
6、EmailField()#存电子邮件-varchar
7、FLoatField()
8、ImageField()#存图片路径-varchar
9、IntegerField()
10、URLField()#存网站地址-varchar
11、TextField()#存大量数据-text

字段选项(Field Options)
max_length
指定数据的最大长度
default
为当前属性（字段）指定默认值
null
指定当前属性（字段）是否允许为空，默认是false

模型中的CRUD
所有的操作均在视图中(Views)执行
通过ORM向DB中增加数据
Entry.objects.creat(属性=值，属性=值)
Entry：具体要操作的Models类

查询指定列操作
语法：value('列1','列2',.\.\.)
用法：Entry.objects.values('列1','列2')
返回：QuerSet

注意：
values()可以用在所有的查询结果集的方法的后面
Author.objects.alL().values('列1'，'列2')

order_by()
作用：排序
语法：order_by('-列1'，'列2'，...)
	列前加"-"表示降序
用法：
Entry.objects.order_by()
Entry.objects.al().order_by(')

根据条件查询部分行数据（重难点）
语法：filter(参数)
用法：Entry.objects.filte(参数)

通过Filed Lookups(查询谓词)完成复杂条件
查询谓词：每个查询谓词都是一个独立的查询条件，可以用在所有的有查询条件的
、位置处
\_\_exact
作用：等值判断
用法：Enty.objects.filter(属性\_\_exact=值)
select from author where id=1
\_\_contains
作用：判断属性中是否包含指定关键字
\_\_lt
作用：判断属性值小于指定值的所有数据
\_\_lte:
作用：判断属性值小于等于指定值的
\_\_gt:
作用：判断属性值大于指定值的
\_\_gte:
作用：判断属性值大于等于指定值的
\_\_startwith
作用：判断属性值是以**开头的
用法：Entry.objects.filter(列__startwith='XX')
sql:select from author where Like xx%'
查询只返回一条数据
语法：get(条件)
用法：Entry.objects.get(查询条件/谓词)
注意：该函数只适用于返回一条记录时使用

修改数据
修改单个数据
通过get()得到要修改的实体对象
通过实体对象的属性修改属性值
再通过实体对象的save()保存回数据库
au Author.objects.get(id=1)
au.name='王宝强'
au.age 35
au.save()

批量修改数据
调用查询结果集的update()即可
Author.objects.aLL).update(属性=值，属性=值)
Author.objects.all().update(age=75)

删除数据
调用实体对象/查询结果集的delete()即可
删除单个对象
obj Author.objects.get(id=1)
obj.delete()
删除多个对象（结果集）
authors Author.objects.all()
authors.delete()

F操作和Q操作
F()
update author set age=age+10
Author.objects.all().update(age=age+10)#错误
作用：用于任执行中获取某列的值
语法：F('列名')
from django.db.models import F
Author.objects.all().update(age=F('age')+10)

Q()
Author.objects.filter(id=1,age=35)
select from author where id=1 and age=35
作用：在查询条件中，可以完成或(or)的操作
语法：
from django.db.models import Q
Q(表达式)|Q〔表达式
\#查询1d为1或年龄大于等于85的人的信息
Author.objects.filter(Q(id=1)Q(age__gte=85))
select from author wehre id=1 or age >85

高级管理
在admin.py中创建高级管理类
定义EntryAdmin类，继承自admin.ModelAdmin
cLass AuthorAdmin(admin.ModelAdmin):
	pass

注册高级管理类
admin.site.register(Entry,EntryAdmin)

允许在EntryAdmin中增加的属性
list_display
作用：指定在列表页中能够显示的字段们
取值：由属性名组成的元组或列表
ex:
List_display ('name','age','email')

List_display_links
作用：定义在列表也中能够连接到详情页的字段们
取值：由属性名组成的元组或列表
注意：取值必须出现在List_display中

list_editable
作用：指定在列表页中就允许修改的字段们
取值：由属性名组成的列表或元组
注意：取值不能出现在list_display_links 中

search_fields
作用：添加允许被搜索的字段们
取值：由属性名组成的元组或列表

list_filter
作用：在列表页的右侧增加过滤器，实现快速筛选
取值：有属性名组成的元组或列表

date_hierarchy
作用：在列表页顶部增加时间选择器，所以取值必须是DateField或DateTimeField的列

fields
作用：在详情页面中，指定显示哪些字段们，并按照什么样的顺序显示
取值：由属性名组成的元组或列表

fieldsets
作用：在详情页面中，对字段们进行分组显示
注意：fieldsets 不能与 fields 共存的
语法：
fieldsets = (
	(
		'分组名称',{
			'fields':('属性1','属性2'),
			'classes':('collapse'),
         }
	),
)

关系映射

1. 一对一映射

   A表中的一条记录只能与B表中的一条记录相关联
   典型代表：一夫一妻制
   数据库中实现：
   	A表：设置主键
   	B表：增加一列，并引用自A表的主键
   语法：属性=models.OneTo0 neField(Entry)
   cLass Wife(models.Model):
   author models.OneToOneField(Author)
   
2. 一对多映射
   A表中的一条数据可以与B表中的任意多条数据关联
   B表中的一条数据只能与A表中的一条数据关联
   在数据库中的体现
   通过　外键 (Foreign Key) 来体现一对多
   在 "多" 表中增加外键(Foreign Key) 对 "一" 表的主键进行引用
   语法
   使用 外键(Foreign Key)
   在 "多" 的实体中，增加：
   属性 = models.ForeignKey(Entry)
   查询
   正向查询 - 通过 Book 查询 Publisher
   book = Book.objects.get(id=1)
   publisher = book.publisher
   反向查询 - 通过 Publisher 查询 Book
   Django默认会在Publisher中增加book_set属性
   pub = Publisher.objects.get(id=1)
   books = pub.book_set.all()

3. 多对多映射
   A表中的一条记录可以与B表中的任意多条记录匹配
   B表中的一条记录可以与A表中的任意多条记录匹配
   在数据库中体现
   必须创建第三张表，关联涉及到的两张数据表
   语法
   在任何一个实体类中，增加：entry = models.ManyToManyField(Entry)
   查询
   正向查询 - 通过 Author 查询所有的 Book
   author = Author.objects.get(id=1)
   book_list = author.book.all()
   通过 关联属性.all() 查询所有的关联数据
   反向查询 - 通过 Book 查询所有的 Author
   	book = Book.objects.get(id=1)

   \#Django 会在 Book 表中增加一个隐式属性 author_set

   ​	author_list = book.author_set.all()

# 项目练习

## 二手车交易平台

1. 判断项目的可行性
2. 分析项目功能
   原型图(ps)
   分析原型图中有哪些功能
   1. 首页
      4个基本模块
      \# 登录、突出、注册、品牌和价格筛选车辆、热卖的车辆
   2. 卖车模块
      \# 填写个人信息，填写车辆基本信息，等待平台审核、允许发布工具
   3. 买方模块
      \# 浏览列表、按品牌和价格筛选车辆，浏览车辆详细信息
   4. 平台管理系统
      审核功能、撮合双方达成交易
   5. 服务保障模块
   6. 个人中心模块
      订单模块、个人最近浏览、个人信息修改、支付信息修改、卖车信息列表、消息审核列表

设计数据库
根据项目功能，思考设计几张表？表与表之间关系是什么？每张表有那些字段？
用户表(UserInfo)：
	用户名(username CharFiled)
	密码(password Char Filed)
	姓名(realname CharField)
	手机号(cellphone CharField)
	身份证号(uidentity CharField)
	地址(address CharField)
	性别(sex IntegerField)\.\.\.
车辆表：车辆型号、车辆名称、上牌日期、发动机号、公里数、维修记录\.\.\.(用户表与车辆表一对多关系)
车辆品牌表：品牌名称、L0G0照片、是否删除\.\.\.(车辆品牌表与车辆表一对多的关系)
订单表：买家、卖家、车辆信息、成交价格、新车价格、订单号、订单状态\.\.\.

创建项目：
django-admin startproject usedcar

根据项目功能划分app
创建用户app：userinfo
创建买家app：buy
创建卖家app：sale
python3 manage.py startapp userinfo
python3 manage.py startapp buy
python3 manage.py startapp sale

创建数据库Usedcar
mysql -uroot -p
create database Usedcar default character set utf8 collate utf8_general_ci;

代码：
usedcar/settings.py

```py
"""
Django settings for usedcar project.

Generated by 'django-admin startproject' using Django 1.11.8.

For more information on this file, see
https://docs.djangoproject.com/en/1.11/topics/settings/

For the full list of settings and their values, see
https://docs.djangoproject.com/en/1.11/ref/settings/
"""

import os

# Build paths inside the project like this: os.path.join(BASE_DIR, ...)
BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))

# Quick-start development settings - unsuitable for production
# See https://docs.djangoproject.com/en/1.11/howto/deployment/checklist/

# SECURITY WARNING: keep the secret key used in production secret!
SECRET_KEY = '^w!ly1(5705)$ai6lc+o_#36v1xkk421&4(pi&o+wq@z5+a080'

# SECURITY WARNING: don't run with debug turned on in production!
DEBUG = True

ALLOWED_HOSTS = ['*']

# Application definition

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'userinfo',
    'buy',
    'sale',
    'front',
]

MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]

ROOT_URLCONF = 'usedcar.urls'

TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]

WSGI_APPLICATION = 'usedcar.wsgi.application'

# Database
# https://docs.djangoproject.com/en/1.11/ref/settings/#databases

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'Usedcar',
        'USER': 'bf',
        'HOST': '127.0.0.1',
        'POST': 3306,
        'PASSWORD': 'q.727626',
    }
}

# Password validation
# https://docs.djangoproject.com/en/1.11/ref/settings/#auth-password-validators

AUTH_PASSWORD_VALIDATORS = [
    {
        'NAME': 'django.contrib.auth.password_validation.UserAttributeSimilarityValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.MinimumLengthValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.CommonPasswordValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.NumericPasswordValidator',
    },
]

# Internationalization
# https://docs.djangoproject.com/en/1.11/topics/i18n/

LANGUAGE_CODE = 'zh-Hans'

TIME_ZONE = 'Asia/Shanghai'

USE_I18N = True

USE_L10N = True

USE_TZ = True

AUTH_USER_MODEL = 'userinfo.Userinfo'

# Static files (CSS, JavaScript, Images)
# https://docs.djangoproject.com/en/1.11/howto/static-files/

STATIC_URL = '/static/'
STATICFILES_DIRS = (BASE_DIR, 'static')

MEDIA_ROOT = os.path.join(BASE_DIR, 'media').replace('\\', '/')
MEDIA_URL = '/media/'
```

usedcar/\__init__.py
```py
import pymysql

pymysql.install_as_MySQLdb()
```

userinfo/models.py

```py
from django.db import models
from django.contrib.auth.models import AbstractUser

SEX_CHOICES = (
    (0, "男"),
    (1, "女"),
)


# Create your models here.
class UserInfo(AbstractUser):
    realname = models.CharField(max_length=30, null=True, verbose_name='姓名')
    cellphone = models.CharField(max_length=11, null=True, verbose_name='手机号')
    uidentity = models.CharField(max_length=18, null=True, verbose_name='身份证号码')
    address = models.CharField(max_length=150, null=True, verbose_name='地址')
    sex = models.IntegerField(choices=SEX_CHOICES, default=0, verbose_name='性别')

    def __str__(self):
        return self.username

    class Meta:
        db_table = 'Users'
        verbose_name = "用户信息表"
        verbose_name_plural = verbose_name
```

python3 manage.py makemigrations
python3 manage.py migrate

sale/models.py
```py
from django.db import models
from userinfo.models import UserInfo

IS_RECORD = (
    (0, "否"),
    (1, "是"),
)

FOR_EXAMINE = (
    (1, '审核中'),
    (2, '审核通过'),
    (3, '审核不通过'),
)


# Create your models here.
class Brand(models.Model):
    btitle = models.CharField(max_length=30, null=False, verbose_name='品牌')
    logo_brand = models.ImageField(upload_to='img/logo', default='logo.png', verbose_name='品牌logo')
    isDelte = models.BooleanField(default=False, verbose_name='是否删除')

    def __str__(self):
        return self.btitle

    class Meta:
        db_table = "Brand"
        verbose_name = "车辆品牌表"
        verbose_name_plural = verbose_name


class Carinfo(models.Model):
    serbran = models.CharField(max_length=30, null=False, verbose_name="车辆型号")
    ctitle = models.CharField(max_length=50, null=False, verbose_name='车辆名称')
    regist_date = models.DateField(default=False, verbose_name='上牌日期')
    engine_no = models.CharField(max_length=50, null=False, verbose_name='发动机编号')
    mileage = models.IntegerField(default=10, verbose_name='公里数')
    is_record = models.IntegerField(choices=IS_RECORD, default=0, verbose_name='是否维修')
    price = models.DecimalField(max_digits=8, decimal_places=2, verbose_name='期望售价')
    new_price = models.DecimalField(max_digits=8, decimal_places=2, verbose_name='新车价格')
    picutre = models.ImageField(upload_to='img/car', default='normal.png', verbose_name='车辆照片')
    formalites = models.IntegerField(choices=IS_RECORD, default=1, verbose_name='手续')
    debt = models.IntegerField(choices=IS_RECORD, default=0, verbose_name='债务')
    promise = models.TextField(null=True, verbose_name='卖家承诺')
    examine = models.IntegerField(choices=FOR_EXAMINE, default=1, verbose_name='审核进度')
    is_purchase = models.BooleanField(default=False, verbose_name='是否购买')
    isDelte = models.BooleanField(default=False, verbose_name='是否删除')
    brand = models.ForeignKey(Brand, verbose_name='车辆品牌')
    user = models.ForeignKey(UserInfo, verbose_name='用户')

    def __str__(self):
        return self.ctitle

    def get_absolute_url(self):
        return '/sale/detail?carid={}'.format(self.id)

    class Meta:
        db_table = 'Carinfo'
        verbose_name = "车辆信息表"
        verbose_name_plural = verbose_name
```

python3 manage.py makemigrations
python3 manage.py migrate
python3 manage.py createsuperuser

userinfo/admin.py

```py
from django.contrib import admin
from .models import *

# Register your models here.
admin.site.register(UserInfo)
```

sale/admin.py
```py
from django.contrib import admin
from .models import *

# Register your models here.
admin.site.register(Brand)
admin.site.register(Carinfo)
```

注册和登录
注册功能

1. html页面：用户名和密码的文本框，用户输入用户名和密码
2. 数据需提交到后台(接收、处理、保存)
3. 返回结果

python3 manage.py startapp front
配置settings.py

front/templates/register.html
```html
<!DOCTYPE html>
<html lang="en">
    <head>
        {% load static %}
        <meta charset="UTF-8">
        <title>Title</title>
    </head>
    <body>
        <form action="{% url 'register_in' %}" method="post">
            {% csrf_token %}
            <p>
                <b>用户注册</b>　<b>{{message}}</b>
            </p>
            <p>
                用户名: <input type="text" name="username" placeholder="请输入用户名">
            </p>
            <p>
                密码: <input type="password" name="userpwd" placeholder="请输入密码">
            </p>
            <p>
                确认密码: <input type="password" name="reuserpwd" placeholder="请输入确认密码">
            </p>
            <p>
                <button name="toregister">注册</button>
            </p>
        </form>
    </body>
</html>
```

usedcar/urls.py
```py
from django.conf.urls import url, include
from django.contrib import admin
from sale import views
from django.conf.urls.static import static
from django.conf import settings


urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^$', views.index, name='index'),
    url(r'^user/', include('userinfo.urls')),
    url(r'^sale/', include('sale.urls')),
	url(r'^buy/', include('buy.urls')),
] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

userinfo/urls.py
```py
from django.conf.urls import url
from userinfo import views


urlpatterns = [
    url(r'register/', views.register, name='register'),
    url(r'registerin/', views.register_, name='register_in'),
    url(r'login/', views.login, name='login'),
    url(r'loginin/', views.login_, name='loginin'),
    url(r'infomes/$', views.infomes, name='infomes'),
    url(r'infomesin/', views.infomes_, name="infomes_in"),
    url(r'logout/', views.logout, name='logout'),
]
```

userinfo/views.py
```py
from django.shortcuts import render, redirect
from .models import *
from sale.models import *
from django.contrib.auth.hashers import make_password, check_password
from django.contrib import auth

# Create your views here.

#　显示注册页面


def register(request):
    return render(request, 'register.html')


#　完成注册的功能
def register_(request):
    user_name = request.POST.get("username")
    olduser = UserInfo.objects.filter(username=user_name)
    if len(olduser) > 0:
        return render(request, 'register.html', {'message': '用户名已经存在'})
    user_pwd = request.POST.get("userpwd")
    re_user_pwd = request.POST.get("reuserpwd")
    if user_pwd != re_user_pwd:
        return render(request, 'register.html', {'message': '两次输入的密码不一致'})
    # 加密
    user_pwd = make_password(user_pwd, 'MarcelArhut', 'pbkdf2_sha1')
    new_user = UserInfo()
    new_user.username = user_name
    new_user.password = user_pwd
    new_user.save()
    return render(request, 'login.html')


# 显示登录界面
def login(request):
    return render(request, 'login.html')


# 登录功能
def login_(request):
    user_name = request.POST.get("username")
    user_pwd = request.POST.get("userpwd")
    user = auth.authenticate(username=user_name, password=user_pwd)
    if user is not None:
        auth.login(request, user)
        request.META['username'] = user_pwd
        return redirect('/')
    else:
        return render(request, 'login.html', {'message': "登录失败"})


def infomes(request):
    return render(request, 'info-message.html')


def infomes_(request):
    brands = request.POST.getlist("brands")[0]
    model = request.POST.get("model")
    ctitle = request.POST.get("ctitle")
    regist_date = request.POST.get("regist_date")
    engineNo = request.POST.get("engineNo")
    mileage = request.POST.get("mileage")
    isService = request.POST.getlist("isService")[0]
    if isService:
        isService = 1
    else:
        isService = 0
    price = request.POST.get("price")
    newprice = request.POST.get("newprice")
    picture = request.FILES.get("picture")
    formalities = request.POST.getlist("formalities")[0]
    if formalities:
        formalities = 1
    else:
        formalities = 0
    isDebt = request.POST.getlist("isDebt")[0]
    if isDebt:
        isDebt = 1
    else:
        isDebt = 0
    promise = request.POST.get("promise")
    car = Carinfo()
    car.serbran = model
    car.ctitle = ctitle
    car.regist_date = regist_date
    car.engine_no = engineNo
    car.mileage = mileage
    car.is_record = isService
    car.price = price
    car.new_price = newprice
    car.picutre = picture
    car.formalites = formalities
    car.debt = isDebt
    car.promise = promise
    # 查询品牌
    brand = Brand.objects.get(btitle=brands)
    car.brand = brand
    # 用户qwer
    user = UserInfo.objects.get(username="qwer")
    car.user = user
    car.save()
    print(brands, model, ctitle, regist_date, engineNo)
    print(mileage, isService, price, newprice)
    print(picture, formalities, isDebt, promise)
    return render(request, 'info-message.html')


def logout(request):
    auth.logout(request)
    return redirect("/")
```

front/templates/login.html
```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
    </head>
    <body>
        <form action="{% url 'loginin' %}" method="post">
            {% csrf_token %}
            <p>
                <b>用户登录</b> <b>{{message}}</b>
            </p>
            <p>
                用户名: <input type="text" name="username" placeholder="请输入用户名">
            </p>
            <p>
                密码: <input type="password" name="userpwd" placeholder="请输入密码">
            </p>
            <p>
                <button name="login">登录</button>
            </p>
    	</form>
    </body>
</html>
```

python3 manage.py makemigrations
python3 manage.py migrate
python3 manage.py createsuperuser

首页展示：
车辆照片、品牌、车名、期望售价、行驶公里数

1. 在数据库中添加一个车辆品牌、一个车辆信息
2. views中查询数据库中的车辆信息，返回给前端的页面
3. templates中搭建页面，展示给后端返回回来的车辆信息
4. 配置url地址

front/templates/index.html

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        {% load static %}
        <meta charset="UTF-8">
        <title>Title</title>
    </head>
    <body>
        <div align="right">
            <p>
                {% if request.user.username %}
                欢迎您:{{request.user.username}}
                <a href="{% url 'logout' %}">退出</a>
                {% else %}
                <a href="{% url 'register' %}">注册</a>|
                <a href="{% url 'login' %}">登录</a>
                {% endif %}
            </p>
        </div>
        <div>
            <a href="{% url 'search' %}?low_price=0&high_price=10"><span name="price">0W-10W</span></a>
            <a href="{% url 'search' %}?low_price=10&high_price=20"><span>10W-20W</span></a>
            <span>20W-40W</span>
            <span>40W-80W</span>
            <span>80W-130W</span>
            <span>130W+</span>
        </div>
        {% for car in carlist.carlist %}
        <p>
            <img src="{{ car.picutre.url }}" alt="">
        </p>
        <p>
            车辆品牌: {{car.brand}}
        </p>
        <p>
            车辆名称: {{car.ctitle}}
        </p>
        <p>
            期望售价: {{car.price}}
        </p>
        <p>
            行驶公里数: {{car.mileage}}
        </p>
        <p>
            <a href="{{car.get_absolute_url}}">查看详情</a>
        </p>
        {% endfor %}
	</body>
</html>
```

sale/views.py
```py
from django.shortcuts import render
from .models import *
import random
from userinfo.models import *
from buy.models import PreOrder


# Create your views here.


def index(request):
    # 同一品牌下的5辆车的基本信息
    # 在平台展示的车辆是审核通过的、未购买、未删除的车辆
    carlist = Carinfo.objects.filter(examine=2, is_purchase=False, isDelte=False)
    carlist = random.sample(list(carlist), 3)
    return render(request, 'index.html', {'carlist':locals()})


def detail(request):
    car_id = request.GET.get("carid")
    car = Carinfo.objects.filter(id=car_id)[0]
    return render(request, 'detail.html', {'carinfo':car})


def pre_buy(request):
    return render(request, 'pre-buy.html')


def pre_buy_(request):
    username = request.user
    cartitle = request.POST.get("cartitle")
    realname = request.POST.get("realname")
    phone = request.POST.get("phone")
    address = request.POST.get("address")
    user = UserInfo.objects.filter(username = username)[0]
    car = Carinfo.objects.filter(ctitle=cartitle)[0]
    user.realname = realname
    user.cellphone = phone
    user.save()
    preorder = PreOrder()
    preorder.user = user
    preorder.car = car
    preorder.address = address
    preorder.save()
    return render(request, 'pre-buy.html', {'message':'预约成功'})


def search_price(request):
    low_price = int(request.GET.get("low_price"))
    high_price = int(request.GET.get("high_price"))
    Carinfo.objects.filter(price__gte=low_prive, price__lte=high_price)
```

front/templates/info-message.html
```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
    </head>
    <body>
        <form action="{% url 'infomes_in' %}" method="post" 　class="form" enctype="multipart/form-data">
            {% csrf_token %}
            <div>
                <p>
                    <b>车辆品牌</b>
                    <select name="brands" id="brands">
                        <option value="奔驰">奔驰</option>
                        <option value="宝马">宝马</option>
                        <option value="奥迪">奥迪</option>
                    </select>
                </p>
                <p>
                    <b>车辆型号</b>
                    <input type="text" name="model">
                </p>
                <p>
                    <b>车辆名称</b>
                    <input type="text" name="ctitle">
                </p>
                <p>
                    <b>车辆上牌日期</b>
                    <input type="text" name="regist_date">
                </p>
                <p>
                    <b>发动机编号</b>
                    <input type="text" name="engineNo">
                </p>
                <p>
                    <b>行驶公里数:</b>
                    <input type="text" name="mileage">
                </p>
                <em>
                    <b>是否有维修记录</b>
                    <input type="radio" name="isService" value="1">是
                    <input type="radio" checked name="isService" value="0">否
                </em>
                <p>
                    <b>期望售价</b>
                    <input type="text" name="price">(万元)
                </p>
                <p>
                    <b>新车价格</b>
                    <input type="text" name="newprice">(万元)
                </p>
                <p>
                    <b>上传车辆照片</b>
                    <input type="file" name="picture">
                </p>
                <em>
                    <b>是否手续齐全</b>
                    <input type="radio" checked name="formalities" value="1">是
                    <input type="radio" name="formalities" value="0">否
                </em>
                <em>
                    <b>是否有债务</b>
                    <input type="radio" name="isDebt" value="1">是
                    <input type="radio" checked name="isDebt" value="0">否
                </em>
                <p>
                    <b>卖家承诺:</b>
                    <textarea name="promise" id="" cols="30" rows="10"></textarea>
                </p>
                <button type="submit">提交</button>
            </div>
        </form>
    </body>
</html>
```

在首页上显示：
如果用户处于登录状态显示欢迎：xxx(用户名)
如果用户处于未登录状态显示：注册|登录

front/templates/detail.html
```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
    </head>
    <body>
        <div>
            <img src="{{carinfo.picutre.url}}" alt="">
        </div>
        <div>
            <p>
                车辆型号:{{carinfo.serbran}}
            </p>
            <p>
                车辆名称:{{carinfo.ctitle}}
            </p>
            <p>
                新车价格:{{carinfo.newprice}}
            </p>
            <p>
                期望售价:{{carinfo.price}}
            </p>
            <p>
                行驶公里数:{{carinfo.mileage}}
            </p>
            <p>
                是否维修:{{carinfo.is_record}}
            </p>
            <p>
                <a href="{% url 'prebuy' %}?carid={{carinfo.id}}">我要试驾</a>
            </p>
        </div>
    </body>
</html>
```

sale/urls.py
```py
from django.conf.urls import url
from sale import views


urlpatterns = [
    url(r'detail', views.detail, name="detail"),
    url(r'prebuy/$', views.pre_buy, name='prebuy'),
    url(r'prebuyin', views.pre_buy_, name='prebuyin'),
    url(r'search', views.search_price, name='search')
]
```

front/templates/pre-buy.html
```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
    </head>
    <body>
        <form action="{% url 'prebuyin' %}" method="post">
            {% csrf_token %}
            <p>
                <b>试驾预约:</b> <b>{{message}}</b>
            </p>
            <p>
                真实姓名: <input type="text" name="realname" placeholder="请输入真实姓名">
            </p>
            <p>
                手机号码: <input type="text" name="phone" placeholder="请输入手机号码">
            </p>
            <p>
                试驾地址: <input type="text" name="address" placeholder="请输入试驾地址">
            </p>
            <p>
                预约车辆: <input type="text" name="cartitle" placeholder="请输入预约车辆">
            </p>
            <button type="submit">提交</button>
        </form>
    </body>
</html>
```

buy/models.py
```py
from django.db import models
from userinfo.models import UserInfo
from sale.models import Carinfo


class PerOrder(models.Model):
    user = models.ForeignKey(UserInfo, verbose_name='用户')
    car = models.ForeignKey(Carinfo, verbose_name='车辆')
    address = models.CharField(max_length=50, null=True, verbose_name="试驾地址")
    is_satis = models.BooleanField(default=True, verbose_name='是否满意')
    evaluate = models.CharField(max_length=150, null=True, verbose_name='评价')
    
    def __str__(self):
        return self.user.username + self.car.ctitle
    
    class Meta:
        db_table = "PreOrder"
        verbose_name = "预约试驾表"
        verbose_name_plural = verbose_name
```

front/templates/prefeedback.html
```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
    </head>
    <body>
        <div>
            <form action="{% url 'precarin' %}" method="post">
                {% csrf_token %}
                <p>
                    <b>预约试驾车辆</b>
                    <input type="text" name="precar" placeholder="请输入预约试驾车辆">
                </p>
                <p>
                    <b>是否满意:</b>
                    <input type="radio" name="istais" value="1" checked>是
                    <input type="radio" name="istais" value="0">否
                </p>
                <p>
                    <b>评价:</b>
                    <textarea name="evaluate" id="" cols="30" rows="10"></textarea>
                </p>
                <p>
                    <button type="submit">
                        提交
                    </button>
                </p>
            </form>
    	</div>
    </body>
</html>
```

buy/views
```py
from django.shortcuts import render, redirect
from userinfo.models import UserInfo
from sale.models import Carinfo
from .models import PreOrder


def pre_feedback(request):
    return render(request, 'prefeedback.html')


def pre_feedback_(request):
    user = request.user
    precar = requset.POST.get("precar")
    istais = request.POST.getlist("istais")[0]
    evaluate = request.POST.get("evaluate")
    user = UserInfo.objects.get(username=user)
    car = Carinfo.objects.get(ctitle=precar)
    preorder = PreOrder.objects.filter(user=user, car=car)[0]
    preorder.is_satis = istais
    preorder.evaluate = evaluate
    preorder.save()
    return redirect("/")
```

buy/urls.py
```py
from django.conf.urls import url
from buy import views


def pre_urlpatterns = [
    url(r'prefeedback', views.pre_feedback, name='prefeedback'),
    url(r'precar', views.pre_feedback_, name='precarin'),
]
```

buy/admin.py
```py
from django.contrib import admin
from .models import *


admin.site.register(PreOrder)
```
