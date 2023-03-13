[toc]

# SPIDER

# 一、爬虫原理

## 1）爬虫概述

* 爬虫定义

  网络蜘蛛、网络机器人，抓取网络数据的程序；

  本质为用Python程序模仿用户点击浏览器并访问网站，而且模仿的越逼真越好。

* 爬取数据目的

  1. 公司项目测试数据
  2. 数据分析和数据挖掘

* 企业获取数据方式

  1. 公司自有数据
  2. 第三方数据平台购买（数据堂、贵阳大数据交易所等）
  3. 爬虫爬取数据

* 各种语言爬虫特点

  1. Python ：请求模块、解析模块丰富成熟,强大的Scrapy网络爬虫框架
  2. JAVA：代码笨重,代码量大
  3. C/C++：虽然效率高,但是代码成型慢

## 2）爬虫分类

* 通用网络爬虫

  * 说明：<font color=red>搜索引擎使用，必须遵守robots协议。</font>
  * robots协议：网站通过robots协议告诉搜索引擎哪些页面可以抓取,哪些页面不能抓取，通用网络爬虫需要遵守robots协议（君子协议）.。
  * 查看网站robots协议示例：`https://www.baidu.com/robots.txt`

* 聚焦网络爬虫

  自己写的爬虫程序，面向主题的爬虫，面向需求的爬虫。



# 二、基础爬虫

## 1. 请求模块requests

### 1）关于requests模块

* 说明

  requests模块是非常优秀的第三方请求模块，支持多种HTTP METHOD请求，是爬虫中使用最多的第三方请求模块。

- 安装

  `pip3 install requests`

### 2）常用方法及属性

#### 2.1 get()

* 作用

  向目标网站发起GET请求，并获取响应对象

* 参数

  1. url ：需要抓取的URL地址 - 字符串
  2. headers : 请求头 - 字典

* 示例

  ```python
  """
  	向京东官网发请求，并获取网站的响应内容
  """
  import requests
  
  response = requests.get(url="https://www.jd.com/")
  html = response.text
  print(html)
  ```

#### 2.2 响应对象属性

* 响应对象获取

  `response = requests.get(url="URL地址", headers={请求头})`

* 属性

  1. text：获取响应内容，结果为字符串
  2. content：获取响应内容，结果为字节串
  3. status_code：获取响应状态码

#### 2.3 基于请求头反爬

- 思考

  ==网站如何来判定是人类正常访问还是爬虫程序访问？--检查请求头！！！== 

  <font color=red>测试网站：http://httpbin.org/get</font>

  ```python
  # 向测试网站http://httpbin.org/get发请求，查看请求头(User-Agent)
  import requests
  
  url = 'http://httpbin.org/get'
  html = requests.get(url=url).text
  print(html)
  ```

  <font color=blue>**以上案例中，请求头中User-Agent为 python-requests/X.XX.0，通过此头网站会第一时间监测到我们为爬虫，并非正常用户请求。**</font>

- 应对基于User-Agent反爬

  **发送请求时包装请求头中的User-Agent，requests.get()中携带 headers 参数！**

  ```python
  """
  	包装好请求头后,向测试网站发请求,并验证
  	养成好习惯，发送请求携带请求头，重构User-Agent
  """
  import requests
  
  url = 'http://httpbin.org/get'
  headers = {'User-Agent':'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/535.1 (KHTML, like Gecko) Chrome/14.0.835.163 Safari/535.1'}
  html = requests.get(url=url,headers=headers).text
  # 在html中确认User-Agent
  print(html)
  ```

#### 2.4 链家二手房爬虫

* 拼接URL地址

  1. 翻页定位规律

  2. 拼接多页URL地址

     * 链家官网地址：https://bj.lianjia.com/

     * 拼接多页URL地址

       ```python
       for page in range(1, 101):
           url = f"https://bj.lianjia.com/ershoufang/pg{page}/"
           print(url)
       ```

* 链家二手房案例

  ```python
  
  ```



## 2. 解析模块lxml+xpath

### 1）安装浏览器插件

* 浏览器：<font color=red>Chrome浏览器</font>

* 安装步骤

  Chrome浏览器 - 右上角设置 - 更多工具 - 扩展程序 - 打开右上角开发者模式开关 - 拖拽插件压缩包到浏览器页面

* 常用浏览器插件

  1. Xpath Helper：可轻松获取html元素的xpath路径

     <font color=red>打开|关闭此插件快捷键：Ctrl + Shift + x</font>

  2. JsonView：格式化输出json数据

### 2）xpath解析

#### 2.1 xpath定义

​	XPath即为XML路径语言，它是一种用来确定XML文档中某部分位置的语言，同样适用于HTML文档的检索

#### 2.2 匹配演示

<font color=red>匹配猫眼电影top100数据：https://maoyan.com/board/4</font>

```python
1) 查找所有的dd节点
    //dd
2) 获取所有电影的名称的a节点: 所有class属性值为name的a节点
    //p[@class="name"]/a
3) 获取dl节点下第2个dd节点的电影节点
    //dl[@class="board-wrapper"]/dd[2]                          
4)获取所有电影详情页链接: 获取每个电影的a节点的href的属性值
    //p[@class="name"]/a/@href
```

* 注意
  1. 只要涉及到条件,加 []，比如：//dl[@class="xxx"]   //dl/dd[1]  //dl/dd[last()]
  2. 只要获取属性值,加 @，比如：//dl[@id="xxx"]   //p/a/@href

#### 2.3 xpath语法

- **选取节点**

  ```python
  【1】// : 从所有节点中查找（包括子节点和后代节点）
  【2】@  : 获取属性值
    2.1> 使用场景1（属性值作为条件）
         //div[@class="movie-item-info"]
    2.2> 使用场景2（直接获取属性值）
         //div[@class="movie-item-info"]/a/img/@src
      
  【3】练习 - 猫眼电影top100
    3.1> 匹配电影名称
        //div[@class="movie-item-info"]/p[1]/a/@title
    3.2> 匹配电影主演
        //div[@class="movie-item-info"]/p[2]/text()
    3.3> 匹配上映时间
        //div[@class="movie-item-info"]/p[3]/text()
    3.4> 匹配电影链接
        //div[@class="movie-item-info"]/p[1]/a/@href
  ```

- 匹配多路径（或）

  `xpath表达式1 | xpath表达式2 | xpath表达式3`

- 常用函数

  1. text()：获取节点文本内容

     xpath表达式末尾不加 /text() :则得到的结果为节点对象

     xpath表达式末尾加 /text() 或者 /@href : 则得到结果为字符串

  2. contains()：匹配属性值中包含某些字符串节点

     匹配class属性值中包含 'movie-item' 这个字符串的 div 节点：`//div[contains(@class,"movie-item")]`

- 总结

  ```python
  1) 列表中存放字符串: ['', '', '', ...]
     xpath表达式的末尾为: /text() 、/@href  得到的列表中为'字符串'
   
  2) 列表中存放节点对象
     其他剩余所有情况得到的列表中均为'节点对象' 
      [<element dd at xxxa>,<element dd at xxxb>,<element dd at xxxc>]
      [<element div at xxxa>,<element div at xxxb>]
      [<element p at xxxa>,<element p at xxxb>,<element p at xxxc>]
  ```


### 3）lxml解析库

#### 1）安装

​	`pip3 install lxml`

#### 2）使用流程

```python
1) 导模块 : 		    from lxml import etree
2) 创建解析对象 ：     eobj = etree.HTML(html)
3) 解析对象调用xpath ：r_list = eobj.xpath('xpath表达式')
```

<font color=red>**注意：只要调用了xpath，得到的结果一定是列表**</font>



#### 3）lxml+xpath使用

```python
1）先使用基准xpath: 匹配所有电影信息的节点对象列表
   //dl[@class="board-wrapper"]/dd
   [<element dd at xxx>,<element dd at xxx>,...]
    
2）再遍历对象列表，依次获取每个电影信息
   item = {}
   for dd in dd_list:
	 	item['name'] = dd.xpath('.//p[@class="name"]/a/text()').strip()
	 	item['star'] = dd.xpath('.//p[@class="star"]/text()').strip()
	 	item['time'] = dd.xpath('.//p[@class="releasetime"]/text()').strip()
```



#### 4）豆瓣图书爬虫

##### 4.1 需求分析

```python
【1】抓取目标 - 豆瓣图书top250的图书信息
    https://book.douban.com/top250?start=0
    https://book.douban.com/top250?start=25
    https://book.douban.com/top250?start=50
    ... ...
    
【2】抓取数据
	2.1) 书籍名称 ：红楼梦
	2.2) 书籍描述 ：[清] 曹雪芹 著 / 人民文学出版社 / 1996-12 / 59.70元
	2.3) 书籍评分 ：9.6
	2.4) 评价人数 ：286382人评价
	2.5) 书籍类型 ：都云作者痴，谁解其中味？

```

##### 4.2 实现流程

```python
【1】确认数据来源 - 响应内容存在
【2】分析URL地址规律 - start为0 25 50 75 ...
【3】xpath表达式
    3.1) 基准xpath,匹配每本书籍的节点对象列表
         //div[@class="indent"]/table
         
    3.2) 依次遍历每本书籍的节点对象，提取具体书籍数据
		书籍名称 ： .//div[@class="pl2"]/a/@title
		书籍描述 ： .//p[@class="pl"]/text()
		书籍评分 ： .//span[@class="rating_nums"]/text()
		评价人数 ： .//span[@class="pl"]/text()
		书籍类型 ： .//span[@class="inq"]/text()
```

##### 4.3 代码实现

```python
"""
	lxml + xpath 抓取豆瓣图书top250图书信息
"""
import requests
from lxml import etree
import time
import random


class BookSpider:
    def __init__(self):
        self.url = 'https://book.douban.com/top250?start={}'

    def get_html(self, url):
        """请求+解析+数据处理"""
        headers = {'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/99.0.4844.84 Safari/537.36'}
        html = requests.get(url=url, headers=headers).text
        # 直接调用解析函数
        self.parse_html(html)

    def parse_html(self, html):
        """lxml + xpath解析提取数据"""
        # 1.创建解析对象
        eobj = etree.HTML(html)
        table_list = eobj.xpath('//table')
        for table in table_list:
            """书的信息要一个一个提取"""
            item = {}
            # 名称
            title_list = table.xpath('.//div[@class="pl2"]/a/@title')
            item['title'] = title_list[0] if title_list else None
            # 信息
            info_list = table.xpath('.//p[@class="pl"]/text()')
            item['info'] = info_list[0] if info_list else None
            # 评分
            score_list = table.xpath('.//span[@class="rating_nums"]/text()')
            item['score'] = score_list[0] if score_list else None
            # 评价人数
            commit_list = table.xpath('.//span[@class="pl"]/text()')
            item['commit'] = commit_list[0][1:-1].strip() if commit_list else None
            # 描述
            comment_list = table.xpath('.//span[@class="inq"]/text()')
            item['comment'] = comment_list[0] if comment_list else None
            print(item)

    def crawl(self):
        for page in range(1, 11):
            start = (page - 1) * 25
            page_url = self.url.format(start)
            self.get_html(url=page_url)
            # 控制数据抓取频率
            time.sleep(random.uniform(0, 2))


if __name__ == '__main__':
    spider = BookSpider()
    spider.crawl()
```

## 3. 数据持久化

### 1）MySQL数据持久化

<font color=red>**pymysql回顾**</font>

- **MySQL建库建表**

  ```mysql
  create database bookdb charset utf8;
  use bookdb;
  create table booktab(
  book_title varchar(100),
  book_info varchar(100),
  book_score varchar(100),
  book_number varchar(100),
  book_desc varchar(500)
  )charset=utf8;
  ```

- **pymysql示例**

  ```python
  import pymysql
  
  db = pymysql.connect(host='localhost',user='root',password='123456',database='bookdb',charset='utf8')
  cursor = db.cursor()
  
  ins = 'insert into booktab values(%s,%s,%s,%s,%s)'
  book_li = ['红楼梦', '[清] 曹雪芹 著 / 人民文学出版社 / 1996-12 / 59.70元', '9.6', ' ( 352409人评价 )', '都云作者痴，谁解其中味？']
  cursor.execute(ins, book_li)
  
  db.commit()
  cursor.close()
  db.close()
  ```

<font color=red>**练习: 将豆瓣图书案例数据持久化到MySQL数据库**</font>



### 2）CSV数据持久化

* 作用

  将爬取的数据存放到本地的csv文件中。

* 依赖模块

  Python标准库模块：csv

* 使用流程

  1. 打开csv文件
  2. 初始化写入对象
  3. 写入数据(参数为列表)

* 示例代码

  ```python
  import csv 
  
  with open('douban_book.csv','w') as f:
      writer = csv.writer(f)
      writer.writerow(["", "", ""])
  ```

<font color=red>**练习:豆瓣图书案例CSV持久化**</font>



# 三、增量爬虫

### 1）多级页面爬虫

* 项目需求

  ```python
  【1】爬取地址
  	https://www.dytt8.net/html/gndy/dyzz/index.html
         
  【2】爬取分析
      *********一级页面需抓取***********
          1、电影详情页链接
          
      *********二级页面需抓取***********
          1、电影名称
          2、电影磁力链接
  ```

* 实现流程

  ```python
  【1】确认数据来源 - 响应内容中存在所抓取数据!!!
  【2】找URL地址规律
      第1页: https://www.dytt8.net/html/gndy/dyzz/list_23_1.html
      第2页: https://www.dytt8.net/html/gndy/dyzz/list_23_2.html
      第n页: https://www.dytt8.net/html/gndy/dyzz/list_23_n.html
  ```

* 代码实现

  ```python
  """
      多级页面数据抓取
  """
  import time
  import random
  import requests
  from lxml import etree
  
  
  class DyttSpider:
      def __init__(self):
          self.url = "https://www.dytt8.net/html/gndy/dyzz/list_23_{}.html"
          self.headers = {"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/98.0.4758.102 Safari/537.36"}
  
      def get_html(self, url):
          # 请求功能函数: 发请求获取html
          # 网站字符编码：右键-查看网页源代码-charset=gb2312
          return requests.get(url=url, headers=self.headers).content.decode("gb2312", "ignore")
  
      def xpath_func(self, html, x):
          # 解析功能函数: xpath解析
          eobj = etree.HTML(html)
  
          return eobj.xpath(x)
  
  
      def crawl(self, url):
          # 爬虫逻辑函数
          first_html = self.get_html(url=url)
          first_x = '//td[@height="26"]/b/a/@href'
          # href_list: ["/html/62340.html", "", ...]
          href_list = self.xpath_func(first_html, first_x)
          for href in href_list:
              # 独立函数：获取详情页数据
              self.get_detail_page(href)
              # 控制爬取频率
              time.sleep(random.randint(1, 3))
  
      def get_detail_page(self, href):
          # 获取详情页数据
          second_html = self.get_html(url="https://www.dytt8.net" + href)
          # 电影名称
          name_x = '//h1/font/text()'
          name_list = self.xpath_func(second_html, name_x)
          name = name_list[0] if name_list else None
          # 磁力链接
          link_x = '//p/a/@href | //a[@target="_blank"]/@href'
          link_list = self.xpath_func(second_html, link_x)
          link = link_list[0] if link_list else None
  
          print(name, link)
  
      def run(self):
          # 爬虫程序入口函数
          for page in range(1, 244):
              page_url = self.url.format(page)
              self.crawl(url=page_url)
              # 控制频率
              time.sleep(random.randint(1, 3))
  
  
  if __name__ == '__main__':
      spider = DyttSpider()
      spider.run()
  ```

### 2）增量爬虫

#### 2.1 增量爬虫概述

```python
【1】引言
	当我们在浏览相关网页的时候会发现，某些网站定时会在原有网页数据的基础上更新一批数据，
	例：1. 某电影网站会实时更新一批最近热门的电影
	    2. 小说网站会根据作者创作的进度实时更新最新的章节数据等等
	当我们在爬虫的过程中遇到时，我们是否需要只爬取网站中最近更新的数据，而不每次都做全量爬虫呢？

【2】概念
	通过爬虫程序监测某网站数据更新的情况，以便可以爬取到该网站更新出的新数据
```

#### 2.2 增量爬虫实现

```python
【1】原理
	1.1》在发送请求之前判断这个URL是不是之前爬取过
    	适用场景：'不断有新页面出现的网站，比如说小说的新章节，每天的最新新闻等'
	1.2》在解析内容后判断这部分内容是不是之前爬取过
    	适用场景：'页面内容会更新的网站'

【2】实现
	2.1》将爬取过程中产生的url进行存储，存储在redis的set中。当下次进行数据爬取时，首先对即将要发起的请求对应的url在存储的url的set中做判断，如果存在则不进行请求，否则才进行请求。
	2.2》对爬取到的网页内容进行唯一标识的制定，然后将该唯一表示存储至redis的set中。当下次爬取到网页数据的时候，在进行持久化存储之前，首先可以先判断该数据的唯一标识在redis的set中是否存在，在决定是否进行持久化存储

```



# 四、动态加载爬虫

## 1）动态加载数据抓取

- 网站特点
  * 鼠标右键，查看网页源码中没有具体数据
  * 滚动鼠标滑轮或其他动作时动态加载
  * 页面局部刷新
- 分析流程
  1. 浏览器F12打开控制台，页面动作抓取网络数据包
  2. 抓取相关json文件的数据
     * 控制台中 XHR ：异步加载的数据包
     * XHR -> QueryStringParameters(查询参数)

## 2）豆瓣电影爬虫

### 2.1 项目需求

```python
【1】地址: 豆瓣电影 - 排行榜 - 剧情
【2】目标: 电影名称、电影评分
```

### 2.2 抓包分析

```python
【1】Request URL(基准URL地址) ：https://movie.douban.com/j/chart/top_list?
【2】Query String(查询参数)
    # 抓取的查询参数如下：
    type: 13 # 电影类型
    interval_id: 100:90
    action: ''
    start: 0  # 每次加载电影的起始索引值 0 20 40 60 
    limit: 20 # 每次加载的电影数量
```

### 2.3 代码实现

```python
"""
抓取豆瓣电影排行榜
在此代码基础上做如下扩展:
1、自动抓取剧情类别下的所有电影(获取电影总数)
2、抓取所有类别的所有电影
   运行效果如下
      请输入电影类别(剧情|喜剧|动作|爱情|....)：爱情
      结果：抓取爱情类别下的所有电影
"""
import requests
import json
import time
import random
import re

class DouBanSpider:
    def __init__(self):
        self.url = 'https://movie.douban.com/j/chart/top_list?type={}&interval_id=100%3A90&action=&start={}&limit=20'

    def get_html(self, url):
        """请求功能函数"""
        headers = {'User-Agent': 'Mozilla/5.0'}
        html = requests.get(url=url, headers=headers).text

        return html

    def parse_html(self, url):
        """爬虫逻辑函数"""
        html = self.get_html(url=url)
        # html: [{},{},...{}]
        html = json.loads(html)
        for one_film_dict in html:
            item = {}
            item['rank'] = one_film_dict['rank']
            item['title'] = one_film_dict['title']
            item['score'] = one_film_dict['score']
            item['time'] = one_film_dict['release_date']
            print(item)

    def get_dic(self):
        """获取所有类别和对应type值的大字典"""
        url = 'https://movie.douban.com/chart'
        html = self.get_html(url=url)
        regex = '<span><a href=.*?type_name=(.*?)&type=(.*?)&interval_id=100:90&action=">'
        # r_list: [('剧情','11'), ('喜剧','23'), ...]
        r_list = re.findall(regex, html, re.S)
        dic = {}
        for r in r_list:
            dic[r[0]] = r[1]

        return dic

    def get_total(self, type_num):
        """获取电影总数量"""
        url = 'https://movie.douban.com/j/chart/top_list_count?type={}&interval_id=100%3A90'.format(type_num)
        html = self.get_html(url=url)
        html = json.loads(html)
        total = html['total']

        return total

    def crawl(self):
        # 大字典：{"剧情":"11", '喜剧':'24', '爱情':'13', ....}
        dic = self.get_dic()
        # 生成显示的菜单
        menu = ''
        for item in dic:
            menu += item + '|'
        print(menu)
        # 让用户选择
        choice = input('请输入电影类别：')
        type_num = dic[choice]
        # 计算此类别电影总数
        total = self.get_total(type_num)
        for start in range(0, total, 20):
            page_url = self.url.format(type_num, start)
            self.parse_html(url=page_url)
            # 控制频率
            time.sleep(random.uniform(0, 1))

if __name__ == '__main__':
    spider = DouBanSpider()
    spider.crawl()
```



# 五、JS加密爬虫

## 1）requests.post()方法说明

* 方法

  `requests.post()`

* 适用场景

  Post类型请求的网站

* 参数说明

  data={} ：请求体数据

* 使用示例

  `res = requests.post(url=url,data=data,headers=headers)`



## 2）控制台抓包

- **打开方式及常用选项**

  ```python
  【1】打开浏览器，F12打开控制台，找到Network选项卡
  
  【2】控制台常用选项
     2.1) Network: 抓取网络数据包
       a> ALL: 抓取所有的网络数据包
       b> XHR：抓取异步加载的网络数据包
       c> JS : 抓取所有的JS文件
     2.2) Sources: 格式化输出并打断点调试JavaScript代码，助于分析爬虫中一些参数
     2.3) Console: 交互模式，可对JavaScript中的代码进行测试
      
  【3】抓取具体网络数据包后
     3.1) 单击左侧网络数据包地址，进入数据包详情，查看右侧
     3.2) 右侧:
       a> Headers: 整个请求信息
          General、Response Headers、Request Headers、Query String、Form Data
       b> Preview: 对响应内容进行预览
       c> Response：响应内容
  ```

## 3）有道翻译爬虫

### 3.1 项目需求

```python
破解有道翻译接口，抓取翻译结果

# 结果展示
请输入要翻译的词语: elephant
翻译结果: 大象
*************************
请输入要翻译的词语: 喵喵叫
翻译结果: mews
```



### 3.2 项目分析流程

```python
【1】准备抓包: F12开启控制台，刷新页面
【2】寻找地址
	2.1) 页面中输入翻译单词，控制台中抓取到网络数据包，查找并分析返回翻译数据的地址
        F12-Network-XHR-Headers-General-Request URL
【3】发现规律
	3.1) 找到返回具体数据的地址，在页面中多输入几个单词，找到对应URL地址
	3.2) 分析对比 Network - All(或者XHR) - Form Data，发现对应的规律
【4】寻找JS加密文件
	控制台右上角 ...->Search->搜索关键字->单击->跳转到Sources，左下角格式化符号{} 
【5】查看JS代码
	搜索关键字，找到相关加密方法，用python实现加密算法
【6】断点调试
	JS代码中部分参数不清楚可通过断点调试来分析查看
【7】Python实现JS加密算法
```



### 3.3 项目步骤

**1、开启F12抓包，找到Form表单数据如下:**

```python
i: 喵喵叫
from: AUTO
to: AUTO
smartresult: dict
client: fanyideskweb
salt: 15614112641250
sign: 94008208919faa19bd531acde36aac5d
ts: 1561411264125
bv: f4d62a2579ebb44874d7ef93ba47e822
doctype: json
version: 2.1
keyfrom: fanyi.web
action: FY_BY_REALTlME

```

**2、在页面中多翻译几个单词，观察Form表单数据变化**

```python
salt: 15614112641250
sign: 94008208919faa19bd531acde36aac5d
ts: 1561411264125
bv: f4d62a2579ebb44874d7ef93ba47e822
# 但是bv的值不变

```

**3、一般为本地js文件加密，刷新页面，找到js文件并分析JS代码**

```python
控制台右上角 - Search - 搜索salt - 查看文件 - 格式化输出

【结果】 : 最终找到相关JS文件 : fanyi.min.js

```

**4、打开JS文件，分析加密算法，用Python实现**

```python
【ts】经过分析为13位的时间戳，字符串类型
   js代码实现)  "" + (new Date).getTime()
   python实现) str(int(time.time() * 1000))

【salt】ts + 0-9之间的随机数(字符串类型)
   js代码实现)  ts + parseInt(10 * Math.random(), 10);
   python实现)  ts + str(random.randint(0, 9))

【sign】（'设置断点调试，来查看 e 的值，发现 e 为要翻译的单词'）
   js代码实现) n.md5("fanyideskweb" + e + salt + "Y2FYu%TNSbMCxc3t2u^XT")
   python实现)
   from hashlib import md5
   m = md5()
   m.update(string.encode())
   sign = m.hexdigest()

```

**5、pycharm中正则处理headers和formdata**

```python
【1】pycharm进入方法 ：Ctrl + r ，选中 Regex
【2】处理headers和formdata
    (.*): (.*)
    "$1": "$2",
【3】点击 Replace All
```



### 3.4 代码实现

```python
import json
import requests
import time
import random
import hashlib


class YdSpider:
    def __init__(self):
        # F12抓包抓到的POST的地址,绝对不能用浏览器地址栏中的地址
        self.url = "https://fanyi.youdao.com/translate_o?smartresult=dict&smartresult=rule"
        self.headers = {
            # 检查频率较高的如下:
            'Cookie': 'OUTFOX_SEARCH_USER_ID=-1500242984@10.169.0.83; OUTFOX_SEARCH_USER_ID_NCOO=1100946878.431965; YOUDAO_MOBILE_ACCESS_TYPE=1; JSESSIONID=aaaix-Uy6Rkk97pQATz9x; ___rl__test__cookies=1646487559497',
            'Referer': 'https://fanyi.youdao.com/',
            'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/99.0.4844.51 Safari/537.36',
        }

    def get_salt_sign_ts(self, word):
        """功能函数:获取salt、sign、ts"""
        # ts
        ts = str(int(time.time() * 1000))
        # salt
        salt = ts + str(random.randint(0, 9))
        # sign
        s = "fanyideskweb" + word + salt + "Ygy_4c=r#e#4EX^NUGUc5"
        m = hashlib.md5()
        m.update(s.encode())
        sign = m.hexdigest()

        return salt, sign, ts

    def get_result(self, word):
        salt, sign, ts = self.get_salt_sign_ts(word)
        data = {
            'i': word,
            'from': 'AUTO',
            'to': 'AUTO',
            'smartresult': 'dict',
            'client': 'fanyideskweb',
            'salt': salt,
            'sign': sign,
            'lts': ts,
            'bv': 'c2777327e4e29b7c4728f13e47bde9a5',
            'doctype': 'json',
            'version': '2.1',
            'keyfrom': 'fanyi.web',
            'action': 'FY_BY_REALTlME',
        }

        html = requests.post(url=self.url, data=data, headers=self.headers).text
        # json.loads():把json格式的字符串转为Python对象(列表、字典)
        html = json.loads(html)

        return html["translateResult"][0][0]["tgt"]

    def crawl(self):
        word = input("请输入要翻译的单词：")
        result = self.get_result(word)
        print(result)


if __name__ == '__main__':
    spider = YdSpider()
    spider.crawl()
```



# 六、selenium爬虫

## 1）selenium概述

```python
【1】定义
    1.1) 开源的Web自动化测试工具
    
【2】用途
    2.1) 对Web系统进行功能性测试,版本迭代时避免重复劳动
    2.2) 兼容性测试(测试web程序在不同操作系统和不同浏览器中是否运行正常)
    2.3) 对web系统进行大数量测试
    
【3】特点
    3.1) 可根据指令操控浏览器
    3.2) 只是工具，必须与第三方浏览器结合使用
    
【4】安装
    4.1) Linux: sudo pip3 install selenium
    4.2) Windows: python -m pip install selenium
```



## 2）环境安装

- **环境安装**

  ```python
  【1】环境说明（'以下三种环境任意安装其中一种即可'）
  	环境一：selenium + chromedriver + Chrome ('我们安装此版本')
  	环境二：selenium + geckodriver + Firefox
  
  【2】提前下载驱动
  	2.1) chromedriver : 下载对应版本
          http://npm.taobao.org/mirrors/chromedriver/
  	2.2) geckodriver
          https://github.com/mozilla/geckodriver/releases
              
  【3】添加到系统环境变量
      sudo cp chromedriver /usr/bin/
          
  【4】修改权限
      sudo chmod 777 /usr/bin/chromedriver
      
  【5】验证
  	5.1) Ubuntu
  		from selenium import webdriver
  		webdriver.Chrome()
  		webdriver.Firefox()
  
  	5.2) Mac
  		from selenium import webdriver
  		webdriver.Chrome(executable_path='/Users/xxx/chromedriver')
  		webdriver.Firefox(executable_path='/User/xxx/geckodriver')
  ```

## 3）selenium详解

### 3.1 代码演示

```python
"""
使用selenium打开浏览器,进入百度的搜索页面
"""
# 导入selenium的webdriver接口
from selenium import webdriver

# 1.打开浏览器 - 创建浏览器对象
driver = webdriver.Chrome()
# 浏览器窗口最大化
driver.maximize_window()
# 2.在地址栏输入 百度的URL地址
driver.get(url='http://www.baidu.com/')
# 3.page_source属性：获取HTML结构源码
html = driver.page_source
# quit(): 关闭浏览器
driver.quit()
```

```python
"""示例代码二：打开百度，搜索高圆圆，点击搜索，查看"""

from selenium import webdriver

# 1. 打开浏览器,输入百度URL地址
driver = webdriver.Chrome()
driver.get(url='http://www.baidu.com/')
# 2. 找到搜索框节点,并发送关键字：高圆圆
node = driver.find_element_by_xpath('//*[@id="kw"]')
node.send_keys('高圆圆')
# 3. 找到 百度一下 按钮,并点击确认
driver.find_element_by_xpath('//*[@id="su"]').click()
```

### 3.2 浏览器对象方法

```python
【1】driver.get(url=url)   - 地址栏输入url地址并确认
【2】driver.quit()         - 关闭浏览器
【3】driver.close()        - 关闭当前页
【4】driver.page_source    - HTML结构源码
【5】driver.page_source.find('字符串')
    从html源码中搜索指定字符串,没有找到返回：-1,经常用于判断是否为最后一页
【6】driver.maximize_window() - 浏览器窗口最大化
```

### 3.3 定位节点

```python
【1】单元素查找('结果为1个节点对象')
    1.1) 【最常用】driver.find_element_by_id('id属性值')
    1.2) 【最常用】driver.find_element_by_name('name属性值')
    1.3) 【最常用】driver.find_element_by_class_name('class属性值')
    1.4) 【最万能】driver.find_element_by_xpath('xpath表达式')
    1.5) 【匹配a节点时常用】driver.find_element_by_link_text('链接文本')
    1.6) 【匹配a节点时常用】driver.find_element_by_partical_link_text('部分链接文本')
    1.7) 【最没用】driver.find_element_by_tag_name('标记名称')
    1.8) 【较常用】driver.find_element_by_css_selector('css表达式')

【2】多元素查找('结果为[节点对象列表]')
    2.1) driver.find_elements_by_id('id属性值')
    2.2) driver.find_elements_by_name('name属性值')
    2.3) driver.find_elements_by_class_name('class属性值')
    2.4) driver.find_elements_by_xpath('xpath表达式')
    2.5) driver.find_elements_by_link_text('链接文本')
    2.6) driver.find_elements_by_partical_link_text('部分链接文本')
    2.7) driver.find_elements_by_tag_name('标记名称')
    2.8) driver.find_elements_by_css_selector('css表达式')
```

### 3.4 节点对象操作

```python
【1】node.send_keys('')  - 向文本框发送内容
【2】node.click()      - 点击
【3】node.get_attribute('属性名')  -  获取节点的属性值
【4】node.text  -  获取当前节点及子节点和后代节点的文本内容
```

## 4）selenium高级

### 4.1 设置无界面模式

```python
from selenium import webdriver

options = webdriver.ChromeOptions()
# 添加无界面参数
options.add_argument('--headless')
driver = webdriver.Chrome(options=options)
```

### 4.2 鼠标操作

```python
"""
鼠标操作三步走：
1、创建鼠标事件类对象
2、指定鼠标行为
3、执行
"""
from selenium import webdriver
# 导入鼠标事件类
from selenium.webdriver import ActionChains

# 1.打开浏览器,输入百度URL地址
driver = webdriver.Chrome()
driver.maximize_window()
driver.get(url='http://www.baidu.com/')
# 2.把鼠标移动到 设置 节点
set_node = driver.find_element_by_xpath('//*[@id="s-usersetting-top"]')
ActionChains(driver).move_to_element(set_node).perform()
# 3.找到 高级搜索 节点,并点击
driver.find_element_by_link_text('高级搜索').click()
```

### 4.3 切换页面

- **适用网站+应对方案**

  ```python
  【1】适用网站类型
      页面中点开链接出现新的窗口，但是浏览器对象driver还是之前页面的对象，需要切换到不同的窗口进行操作
      
  【2】应对方案 - driver.switch_to.window()
      
      # 获取当前所有句柄（窗口）- [handle1,handle2]
      all_handles = driver.window_handles
      # 切换browser到新的窗口，获取新窗口的对象
      driver.switch_to.window(all_handles[1])
  
  ```

- **民政部网站案例-selenium**

  ```python
  """
  适用selenium+Chrome抓取民政部行政区划代码
  http://www.mca.gov.cn/article/sj/xzqh/2020/
  """
  import sys
  
  from selenium import webdriver
  import time
  import redis
  from hashlib import md5
  
  class MzbSpider:
      def __init__(self):
          # 设置无界面
          self.options = webdriver.ChromeOptions()
          self.options.add_argument('--headless')
          self.driver = webdriver.Chrome(options=self.options)
  
          self.driver.get(url='http://www.mca.gov.cn/article/sj/xzqh/2020/')
          self.r = redis.Redis(host='localhost',port=6379,db=0)
  
      def md5_href(self, href):
          """md5加密的功能函数"""
          m = md5()
          m.update(href.encode())
  
          return m.hexdigest()
  
      def parse_html(self):
          """解析提取数据"""
          new_month_a = self.driver.find_element_by_xpath('//*[@id="list_content"]/div[2]/div/ul/table/tbody/tr[1]/td[2]/a')
          # 生成指纹
          href = new_month_a.get_attribute('href')
          finger = self.md5_href(href)
          if self.r.sadd('mzb:spiders', finger) == 1:
              new_month_a.click()
              time.sleep(3)
              # 切换句柄
              li = self.driver.window_handles
              self.driver.switch_to.window(li[1])
              # 开始提取数据(单独写了个函数提取数据而已)
              self.get_data()
          else:
              sys.exit('更新完成')
  
      def get_data(self):
          """提取具体数据"""
          tr_list = self.driver.find_elements_by_xpath('//tr[@height="19"]')
          for tr in tr_list:
              item = {}
              li = tr.text.split()
              item['code'] = li[0]
              item['name'] = li[1]
              print(item)
          # 关闭浏览器
          self.driver.quit()
  
  if __name__ == '__main__':
      spider = MzbSpider()
      spider.parse_html()
  
  ```

### 4.4 切换frame

- **特点+方法**

  ```python
  【1】特点
      网页中嵌套了网页，先切换到iframe，然后再执行其他操作
   
  【2】处理步骤
      2.1) 切换到要处理的Frame
      2.2) 在Frame中定位页面元素并进行操作
      2.3) 返回当前处理的Frame的上一级页面或主页面
  
  【3】常用方法
      3.1) 切换到frame  -  driver.switch_to.frame(frame节点对象)
      3.2) 返回上一级   -  driver.switch_to.parent_frame()
      3.3) 返回主页面   -  driver.switch_to.default_content()
      
  【4】使用说明
      4.1) 方法一: 默认支持id和name属性值
          switch_to.frame(id属性值|name属性值)
      4.2) 方法二:
          a> 先找到frame节点 : frame_node = driver.find_element_by_xpath('xxxx')
          b> 在切换到frame   : driver.switch_to.frame(frame_node)
  
  ```

- **示例1 - 登录豆瓣网**

  ```python
  """
  登录豆瓣网
  """
  from selenium import webdriver
  import time
  
  # 打开豆瓣官网
  driver = webdriver.Chrome()
  driver.get('https://www.douban.com/')
  
  # 切换到iframe子页面
  login_frame = driver.find_element_by_xpath('//*[@id="anony-reg-new"]/div/div[1]/iframe')
  driver.switch_to.frame(login_frame)
  
  # 密码登录 + 用户名 + 密码 + 登录豆瓣
  driver.find_element_by_xpath('/html/body/div[1]/div[1]/ul[1]/li[2]').click()
  driver.find_element_by_xpath('//*[@id="username"]').send_keys('自己的用户名')
  driver.find_element_by_xpath('//*[@id="password"]').send_keys('自己的密码')
  driver.find_element_by_xpath('/html/body/div[1]/div[2]/div[1]/div[5]/a').click()
  time.sleep(3)
  
  # 点击我的豆瓣
  driver.find_element_by_xpath('//*[@id="db-nav-sns"]/div/div/div[3]/ul/li[2]/a').click()
  
  ```

# 七、验证码反爬

## 1）图形验证码

### 1.1 机器视觉概述

```python
【1】作用
    处理图形验证码

【2】三个重要概念 - OCR、tesseract-ocr、pytesseract
    2.1) OCR
        光学字符识别(Optical Character Recognition),通过扫描等光学输入方式将各种票据、报刊、书籍、文稿及其它印刷品的文字转化为图像信息，再利用文字识别技术将图像信息转化为电子文本

    2.2) tesseract-ocr
        OCR的一个底层识别库（不是模块，不能导入），由Google维护的开源OCR识别库

    2.3) pytesseract
        Python模块,可调用底层识别库，是对tesseract-ocr做的一层Python API封装

```

### 1.2 模块安装

- **安装tesseract-ocr**

  ```python
  【1】安装tesseract-ocr
  	1.1》Ubuntu安装 ：sudo apt-get install tesseract-ocr
  	1.2》Windows安装
      	下载安装包，双击安装
      	添加到环境变量(Path)
  【2】测试（终端 | cmd命令行）
  	tesseract xxx.jpg 文件名
  ```

- **安装pytesseract**

  ```python
  【1】安装
      sudo pip3 install pytesseract
      
  【2】使用示例
      import pytesseract
      # Python图片处理库
      from PIL import Image
  
      # 创建图片对象
      img = Image.open('test1.jpg')
      # 图片转字符串
      result = pytesseract.image_to_string(img)
      print(result)
  
  ```

- **面试问题: 如何处理爬虫中遇到的验证码**

  ```
  【1】图形验证码
  	简单的图形验证码,我使用tesseract-ocr去处理
  	对于一些复杂的验证码,我们使用在线打码(图鉴、云打码)
  【2】滑块、缺口验证码
  	使用selenium处理,模拟人的行为去滑动滑块
  	或者使用人工打码
  
  ```

## 2）滑块缺口验证码

### 2.1 豆瓣网登录爬虫

####  1）项目需求

```python
【1】URL地址: https://www.douban.com/
【2】先输入几次错误的密码，让登录出现滑块缺口验证，以便于我们破解
【3】模拟人的行为(总距离：200)
    3.1) 先快速滑动一部分距离(滑动160)
    3.2) 剩余距离(40)：先匀加速(40*4/5=32),再匀减速(40*1/5=8)
【4】详细看代码注释

```

#### 2）项目实现

```python
"""
说明：先输入几次错误的密码，出现滑块缺口验证码
"""
from selenium import webdriver
# 导入鼠标事件类
from selenium.webdriver import ActionChains
import time

# 加速度函数
def get_tracks(distance):
    """
    拿到移动轨迹，模仿人的滑动行为，先匀加速后匀减速
    匀变速运动基本公式：
    ①v=v0+at
    ②s=v0t+½at²
    """
    # 初速度
    v = 0
    # 单位时间为0.3s来统计轨迹，轨迹即0.3内的位移
    t = 0.3
    # 位置/轨迹列表,列表内的一个元素代表0.3s的位移
    tracks = []
    # 当前的位移
    current = 0
    # 到达mid值开始减速
    mid = distance*4/5
    while current < distance:
        if current < mid:
            # 加速度越小,单位时间内的位移越小,模拟的轨迹就越多越详细
            a = 2
        else:
            a = -3

        # 初速度
        v0 = v
        # 0.3秒内的位移
        s = v0*t+0.5*a*(t**2)
        # 当前的位置
        current += s
        # 添加到轨迹列表
        tracks.append(round(s))
        # 速度已经达到v，该速度作为下次的初速度
        v = v0 + a*t
    return tracks
    # tracks: [第一个0.3秒的移动距离,第二个0.3秒的移动距离,...]

# 1、打开豆瓣官网 - 并将窗口最大化
driver = webdriver.Chrome()
driver.get(url='https://www.douban.com/')
driver.maximize_window()

# 2、切换到iframe子页面
iframe_node = driver.find_element_by_xpath('//div[@class="login"]/iframe')
driver.switch_to.frame(iframe_node)
# 3、密码登录 + 用户名 + 密码 + 登录豆瓣
driver.find_element_by_xpath('/html/body/div[1]/div[1]/ul[1]/li[2]').click()
driver.find_element_by_xpath('//*[@id="username"]').send_keys('15110225726')
driver.find_element_by_xpath('//*[@id="password"]').send_keys('aaa')

while True:
    try:
        driver.find_element_by_xpath('/html/body/div[1]/div[2]/div[1]/div[5]/a').click()
        time.sleep(3)

        # 4、切换到新的iframe子页面 - 滑块验证
        driver.switch_to.frame('tcaptcha_iframe')

        # 5、按住开始滑动位置按钮 - 先移动180个像素
        node = driver.find_element_by_xpath('//*[@id="tcaptcha_drag_button"]')
        # click_and_hold(): 鼠标按住某个节点并保持
        ActionChains(driver).click_and_hold(node).perform()
        # 鼠标移动到距离某个节点水平 及 垂直的距离
        ActionChains(driver).move_to_element_with_offset(to_element=node, xoffset=180, yoffset=0).perform()
        # 6、使用加速度函数移动剩下的距离-25个像素
        # tracks: []
        tracks = get_tracks(25)
        for track in tracks:
            # 鼠标移动到距离当前位置水平 及 垂直的距离
            ActionChains(driver).move_by_offset(xoffset=track, yoffset=0).perform()

        # 7、延迟释放鼠标: release()
        time.sleep(0.5)
        # release(): 释放鼠标
        ActionChains(driver).release().perform()
    except:
        pass
```

## 