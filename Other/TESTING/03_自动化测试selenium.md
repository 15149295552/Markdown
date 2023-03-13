[toc]

# 自动化测试

## 一、自动化测试介绍

### 1、什么是自动化测试

概念：由程序代替人工进行系统校验的过程

#### 1.1自动化测试能解决的问题？

* 回归测试
  * 针对之前老的功能进行测试     通过自动化的代码来实现  
  * 针对上一个版本的问题的回归

* 兼容性测试（浏览器兼容性）

* 提高工作效率，保障产品质量   



#### 1.2自动化测试的优点

* 自动化测试能在较少的时间内执行更多的测试用例  

* 自动化测试能够减少人为的错误   

* 自动化测试可以重复执行



#### 1.3自动化则试的误区

* 自动化测试可以完全代替手工测试

* 自动化测试一定比手工测试厉害

* 自动化测试可以发现更多的BUG

* 自动化测试适用于所有的功能

---



### 2、什么是UI自动化测试

概念：UI(user interface)通过对web应用以及app应用进行自动化测试的过程。

####  <font color=red>2.1 什么项目适合做ui自动化测试？</font>

* 需求变动不频繁（前端代码变更维护不方便）
* 项目周期长（项目短，上线之后不需要再去测试）
* 项目需要回归测试（不用回归测试的不需要写自动化）

#### 2.2 UI自动化测试在什么阶段开始？

* 手工测试完成之后才做自动化测试，相当于是编写自动化测试代码

## 二、web自动化测试基础

### 1、web自动化框架

#### 1.1 主流的web自动化工具

* selenium     主要用来做web自动化测试的，开源的免费的工具。

#### 1.2 selenium特点

selenium中文名是硒，就是用来做web自动化测试的

* 开源软件:  源代码开放

* 跨平台:   linux、windows、 mac操作系统

* 支持多种浏览器：firefox、chrome、 ie、edge、opera、safari

* 支持多语言：python\java\C#\js\Ruby\PHP

* 成熟稳定功能强大：被大公司使用。google、华为、百度、腾讯


#### 1.3selenium发展史

* selenium 1.0

  * selenium IDE 

    * 是firefox的一款插件，通过它可以记录用户的操作并生成对应的自动化脚本。

  * selenium grid     通过grid可以将脚本下发到不同的电脑在不同的浏览器上面执行。

  * selenium RC

    由JS封装的一个工具，用来对浏览器进行操作的。

* selenium 2.0 

  * selenium 1.0 + webdriver 
  * 针对浏览器的操作都是通过webdriver来实现的。
  * 支持的语言更多

* selenium 3.0 

  * 删除了selenium RC
  * 支持macOS，支持safari浏览器
  * 支持微软最新的EDGE浏览器，支持更多的浏览器

### <font color=red>2、环境搭建</font>

#### 2.1 浏览器安装（浏览器电脑已安装）

<font color=red>需要注意浏览器与浏览器驱动的版本， 不同的浏览器有不同的浏览器驱动，而且不同的版本也有不同的浏览器驱动</font>

#### 2.2selenium工具包安装

* 在线安装方式:   在终端中输入:  sudo pip3 install selenium

* 离线安装方式:   

  * 需要获取selenium离线安装包并解压
  * 在终端进入到解压的目录，然后执行sudo python3 setup.py install 

#### 2.3 Chrome浏览器驱动安装

* 查看自己浏览器的版本，比如Chrome，打开浏览器，输入：chrome://version

* 通过https://npm.taobao.org/mirrors/chromedriver/   获取对应的浏览器驱动

* 解压浏览器驱动文件，并将驱动文件复制到/usr/bin/目录并给定可执行权限

  sudo cp chromedriver /usr/bin/

  sudo chmod 777 /usr/bin/chromedriver

#### **2.4 Firefox浏览器驱动安装**

https://github.com/mozilla/geckodriver/releases

安装方式同Chrome浏览器驱动器方法


#### 2.4 入门示例

* 项目创建


```python
from selenium import webdriver


# 1.打开浏览器
# 2.输入地址并确认
# 3.定位 搜索框 节点,并发送关键字:赵丽颖
# 4.定位 百度一下 节点,并点击

driver = webdriver.Chrome()
driver.get(url="http://www.baidu.com/")
driver.find_element_by_id("kw").send_keys("赵丽颖")
driver.find_element_by_id("su").click()
```

## 三、元素定位

#### 3.1 如何进行元素定位？

元素： 由标签头+标签尾+标签头和标签尾包括的文本内容

元素的信息就是指元素的标签名以及元素的属性

元素的层级结构就是指元素之间相互嵌套的层级结构

元素定位最终就是通过元素的信息或者元素的层级结构来进行元素定位。

#### 3.2 元素定位

* id定位
* name定位
* class_name定位
* tag_name定位
* link_text定位
* partail_link_text定位
* xpath定位
* css定位

##### <font color=red>3.2.1 ID定位</font>

* 通过元素的ID属性值来进行元素定位  ，在html标准规范中 ID值是唯一的

  说明： 元素要有ID属性

* 定位方法：  find_element_by_id(id)     # id参数表示的是id的属性值

##### 3.2.2 name定位

* 通过元素的name属性值为进行元素定位   name属性值 在HTML页面中，是可以重复的。

  说明：元素要有name属性

* 定位方法:  find_element_by_name(name)   # name 参数表示的是name的属性值

##### <font color=red>3.2.3 class_name定位</font>

* 通过元素的class属性值进行元素定位    class属性值是可重复的

  说明：元素必须要有class属性

* 定位方法： find_element_by_class_name(class_name)   #  class_name参数表示的是class的<font color=red>属性值</font>

##### 3.2.4 tag_name定位

* 通过元素的标签名称进行定位，   在同一个html页面当中，相同标签元素会有很多。

  这种定位元素的方式不建议大家在工作当中使用。

* 定位方法:  find_element_by_tag_name(tag_name)  # tag_name表示的是元素的标签名称。

  如果有重复的元素，定位到的元素默认都是第一个元素

##### 3.2.5  link_text定位

* 通过超链接的全部文本信息进行元素定位 ,主要用来定位a标签
* 定位方法：  find_element_by_link_text(link_text)   #  link_text参数代表的是a标签的<font color=red>全部</font>文本内容。

##### <font color=red>3.2.6 partial_link_text定位</font>

* 通过超链接的局部文本信息进行元素定位，主要用来定位a标签 
* 定位方法：find_element_by_partial_link_text(partial_link_text)    #  partial_link_text表示的是a标签 的<font color=red>局部</font>文本内容

##### 3.2.7 定位一组元素

* 定位一组元素的方法:

  find_elements_by_id(id)    

  find_elements_by_tag_name(tag_name)

* 定位一组元素返回的值是一个列表

* 可以通过下标来使用列表中的元素

* 下标是从0开始。



## 四、元素定位Xpath、CSS

为什么要使用Xpath或者CSS

因为在页面当中，有一些元素无法通过元素的信息精准的定位到，所以需要借助于Xpath和CSS

### 1 xpath元素定位

#### 1.1 什么是xpath

xpath是用来在xml文件中进行元素定位的标记语言，同样适用于html文档的检索

#### <font color=red>1.2 Xpath定位策略</font>

- 路径定位
- 属性定位
- 属性与逻辑结合
- 属性与层级结合

定位方法:  find_element_by_xpath(xpath)   # xpath表达式

按Ctrl+F 可以在搜索框对xpath和css表达式进行校验

##### 1.2.1 路径定位

- 绝对路径   表达式是以  /html开头，元素的层级之间是以  / 分隔

  相同层级的元素可以使用下标，下标是从1开始.

  需要列出元素所经过的所有层级元素  ,  工作当中， 一般不使用绝对路径

  例：/html/body/div/ul/li[1]/a

- 相对路径   匹配任意层级的元素，  是以  //tag_name或者//* 开头

  也可以使用下标，下标是从1开始。

  例子：//ul/li[5]

- 匹配示例

  ```python
  """
  匹配猫眼电影top100：https://maoyan.com/board/4
  """
  【1】查找所有的dd节点
      //dd
  【2】获取所有电影的名称的a节点: 所有class属性值为name的a节点
      //p[@class="name"]/a
      
  【3】获取dl节点下第2个dd节点的电影节点
      //dl[@class="board-wrapper"]/dd[2]                          
  
  【注意】                             
      1> 只要涉及到条件,加 [] : 
          //dl[@class="xxx"]   //dl/dd[2]
          
      2> 只要获取属性值,加 @  : 
          //dl[@class="xxx"]
  ```



##### 1.2.2 xpath语法

```python
【1】// : 从所有节点中查找（包括子节点和后代节点）
【2】@  : 获取属性值
  2.1> 使用场景1（属性值作为条件）
       //div[@class="movie-item-info"]
  2.2> 使用场景2（直接获取属性值）
       //div[@class="movie-item-info"]/a/img
    
【3】练习 - 猫眼电影top100
  3.1> 匹配电影名称
      //div[@class="movie-item-info"]/p[1]/a
  3.2> 匹配电影主演
      //div[@class="movie-item-info"]/p[2]
  3.3> 匹配上映时间
      //div[@class="movie-item-info"]/p[3]
```

##### 1.2.3 常用方法及属性

```python
【1】text属性 ：获取当前节点及子节点和后代节点的文本内容
	node = driver.find_element_by_xpath('//div[@class="item"]')
	node_text = node.text
【2】get_attribute('属性名') : 获取节点的属性值
     node = driver.find_element_by_xpath('//div[@class="item"]')
     href = node.get_attribute('href')
```

##### 1.2.4 XPATH扩展

- //*[text() = 'value']   value表示的是要定位的元素的全部文本内容.   

- //*[contains(@attribute,'value')]   attribute表示的属性名称, value表示的是字符串

  要定位的元素中，attribute属性的属性值包含了value的内容。

- //*[starts-with(@attribute,'value')]      attribute表示的属性名称, value表示的是字符串

  要定位的元素，attribute属性的属性值是以value开头

------

### 2、CSS定位

#### 2.1 什么是CSS

总结：css是可以用来在selenium中定位元素的

CSS定位元素的方法： find_element_by_css_selector(css_selector)   # css_selector表示的是CSS选择器表达式

#### <font color=red>2.2 CSS定位策略</font>

- id选择器
- class选择器
- 元素选择器
- 属性选择器
- 层级选择器

##### 2.2.1 id选择器

- 表达式： #id    #  表示通过元素的ID属性进行元素选择    id  表示的的id属性的属性值

##### 2.2.2 class选择器

- 表达式： .class   # .表示通过元素的class属性进行元素选择， class表示的class的<font color=red>属性值</font>

##### 2.2.3 属性选择器

- 就是通过元素的属性来选择元素。  表达式：[attribute='value']  #attribute 表示的是属性名称，value表示的是属性值 


```python
driver.find_element_by_css_selector("[class='username']").send_keys("123456@qq.com")
```

##### 2.2.4 层级选择器

- 父子层级关系选择 器 
  - 表达式： element1>element2      通过element1来找element2，<font color=red>并且element2是element1的直接子元素</font>
- 隔代层级关系选择器
  - 表达式： element1 element2    通过element1来找element2， <font color=red>并且element2是element1的后代元素</font>

```python
# 通过css的层级选择器定位用户名输入框输入
driver.find_element_by_css_selector(".form-group>#uname").send_keys("admin")
```

### 3、定位元素的另外一种写法

- find_element(By.ID, id)   需要导入By类。

  ```python
  from selenium.webdriver.common.by import By
  
  # 通过css的层级选择器定位用户名输入框输入
  driver.find_element(By.CSS_SELECTOR, ".form-group>#uname").send_keys("admin")
  ```

------



## 五、元素操作及浏览器操作方法

### 1、元素操作

- 点击操作    element.click()      element表示的是元素对象
- 输入操作   element.send_keys("value")    element表示的是元素对象， value表示的是要输入的内容
- 清除操作   element.clear()     element表示的是元素对象. 将输入框里面的内容全部清除。



### 2、浏览器操作

- 浏览器常用操作方法

1. <font color=red>maximize_window() 最大化浏览器窗口 --> 模拟浏览器最大化按钮</font>
2. back() 后退 --> 模拟浏览器后退按
3. forward() 前进 --> 模拟浏览器前进按钮
4. refresh() 刷新 --> 模拟浏览器F5刷新
5. close() 关闭当前窗口 --> 模拟点击浏览器关闭按钮
6. quit() 关闭浏览器驱动对象 --> 关闭所有程序启动的窗口

### 3、获取元素信息

- 为什么要学习获取元素信息的方法

  主要为了获取相关的信息进行断言，判断自动化用例最终的执行结果。

- 获取元素常用的方法：

  - size    获取元素的大小   返回的是一个字典，里面包含 元素高度和宽度的值{‘width’:xx, ‘height’:xx}
  - text    获取元素的文本内容
  - get_attribute("attribute")  获取元素对应属性名称的属性值 ， attribute表示的是属性名


## 六、鼠标和键盘操作

### 1、鼠标操作

`from selenium.webdriver import ActionChains`

#### 1.1 鼠标操作实现方式

- selenium提供鼠标操作的方法及步骤

  需要导入ActionChains类

  - 通过ActionChains实例化鼠标对象   action = ActionChains(driver)  # driver表示的是浏览器驱动对象
  - 调用鼠标的事件方法
  - 调用鼠标的执行方法   action.perform()

#### 1.2 鼠标右击操作

- 右击操作的实现步骤:

  针对由html自定义的右键菜单。可以使用右击的方式来进行操作。

  - 创建鼠标对象   action = ActionChains(driver)
  - 调用右击事件方法  <font color=red>action.context_click(element)</font>  # element表示的是一个元素对象
  - 调用鼠标执行方法  action.perform()

#### 1.3 鼠标双击操作

- 鼠标双击的实现步骤

  - 创建鼠标对象  action=ActionChains(driver)
  - 调用鼠标双击事件方法  action.double_click(element)  # element表示是元素对象
  - 调用鼠标的执行方法 action.perform()


#### 1.4 <font color=red>鼠标悬停操作</font>

- 鼠标悬停实现步骤：
  - 实例化鼠标对象   action = ActionChains(driver)
  - 调用鼠标悬停事件方法  action.move_to_element(element)  # element表示的是元素对象
  - 调用鼠标执行方法 action.perform()

#### 1.5 鼠标单元素拖动操作

- 鼠标单元素拖动实现步骤
  - 创建鼠标对象 action=ActionChains(driver)
  - 调用鼠标单元素拖动事件方法   action.move_to_element_with_offset(element, x, y)  x, y 表示的元素拖动时横向和纵向移动的距离，单位为像素， element表示的是元素对象  移动的像素最终要比在web页面中看到的移动像素值要大，最好大于5个像素或者10像素
  - 调用鼠标执行方法  action.perform()

#### 1.6 示例

```python
"""
    selenium操作鼠标
    鼠标操作三步走,实例化指定行为加执行
"""
from selenium import webdriver
from selenium.webdriver import ActionChains
from selenium.webdriver.common.by import By

driver = webdriver.Chrome()
driver.maximize_window()
driver.get(url='http://www.baidu.com/')

# 右上角 设置 节点
node = driver.find_element(By.XPATH, '//*[@id="s-usersetting-top"]')
# 操作鼠标
ActionChains(driver).move_to_element(node).perform()

# 高级搜索节点
driver.find_element(By.LINK_TEXT, "高级搜索").click()
```

### 2、键盘操作

`from selenium.webdriver.common.key import Keys`

- 模拟键盘上面的快捷键的操作

- 调用键盘操作的快捷键的方法  element.send_keys(快捷键的键值)

  单键值：   直接传入对应的键值

  组合键： 键值之间由逗号分隔

  send_keys(Keys.CONTROL,  Keys.SHIFT,  'i')    

  常用的快捷键

  1. send_keys(Keys.BACK_SPACE) 删除键(BackSpace)
  2. send_keys(Keys.SPACE) 空格键(Space)
  3. send_keys(Keys.TAB) 制表键(Tab)
  4. send_keys(Keys.ESCAPE) 回退键(Esc)
  5. send_keys(Keys.ENTER) 回车键(Enter)
  6. send_keys(Keys.CONTROL,'a') 全选(Ctrl+A)
  7. send_keys(Keys.CONTROL,'c') 复制(Ctrl+C)
  8. send_keys(Keys.CONTROL, 'v') 粘贴

------

## 七、元素等待

HTML加载需要时间，影响HTML加载的因素：

- 服务器性能
- 网络速度
- 本身电脑的配置

### 1、隐式等待

概念：首先要等待整个页面加载完成，再去进行元素定位，如果在定位过程中找到了元素，直接返回该元素，继续后面的操作，如果在指定的时间内没有找到该元素，那么每隔0.5秒再去找，如果超过了指定时间，就会抛出NoSuchElementException的异常错误。

隐式等待实现方式:

driver.implicitlty_wait(timeout)   # timeout表示的是最长的等待时间 单位为S

隐式等待只需要设置一次，对所有的元素定位的方法都是有效的。



### 2、显示等待

概念：定位指定元素时，如果能找到该元素，那么就直接返回该 元素，如果找不到，那么每隔指定的时间再去定位该 元素，如果超出最长等待时间，那么就抛出TimeOutException。

显示等待的实现：

WebDriverWait(driver, timeout, poll_frequency=0.5).until(lambda x:x.find_element(By.ID, "userA"))

WebDriverWait等待类需要导入

driver指的是浏览器驱动对象

timeout表示的是最长等待时间

poll_frequency表示的是检测的间隔时间，默认是0.5

后面跟上until方法，在until方法跟上匿名函数来实现元素定位。



显示等待与隐式等待的区别:

1、抛出的异常不一样 ， 隐式等待超时，报的NoSuchElementException  ，显示等待超时，报的是 TimeOutException

2、作用域不一样， 隐式等待对所有元素定位的方法都有效，只需要定义一次， 显示等待只针对单个元素

3、显示等待不需要等待整个页面的HTML的DOM树加载完成，显式等待的效率更高，工作当中一般使用显示等待。而隐式等待需整个HTML DOM树加载完成。

```python
"""
    显示等待
"""
import time
from selenium import webdriver
from selenium.webdriver.common.by import By
# 显示等待
from selenium.webdriver.support.wait import WebDriverWait


# 1.进入登录页
driver = webdriver.Chrome()
driver.maximize_window()
driver.get(url="http://127.0.0.1:7000/dadashop/templates/login.html")

# 2.点击登录
driver.find_element(By.ID, "username").send_keys("zhaoliying")
driver.find_element(By.CSS_SELECTOR, "#password").send_keys("123456")
driver.find_element(By.XPATH, '//*[@id="bt-login"]').click()

# 3.处理alert[点击确定]
time.sleep(2)
driver.switch_to.alert.accept()

# 4.定位　地址管理节点[显示等待]
x = '//*[@id="top"]/div[3]/ul/li[1]/a/img'
wait = WebDriverWait(driver, 10, 0.2)
wait.until(lambda y: y.find_element(By.XPATH, x)).click()

# 5.定位第一个收货地址
x1 = '//*[@id="container"]/div[2]/div[2]/div/div[2]'
node = wait.until(lambda y: y.find_element(By.XPATH, x1))
print(node.text)
```

### 3、强制等待

- 强制等待就是让代码休眠，不做任何的操作

  time.sleep(time)     单位为time

- 常用的场景：

  - 当要获取元素的文本内容时，而元素的文本内容是需要通过后台接口请求并渲染的，此时，如果使用隐式等待或显示等待是没有办法获取到文本内容，所以需要使用强制等待
  - 当要操作的元素已经存在，但是有其他的元素需要等待且与该操作的元素有业务关联，如果使用隐式等待或显示等待对该元素进行操作的话，是没有办法进行的，也需要使用强制等待。

```python
"""
	登录获取登录结果提示
"""

import time
from selenium import webdriver

driver = webdriver.Chrome()
driver.get(url="http://127.0.0.1:7000/dadashop/templates/login.html")
driver.find_element_by_id("username").send_keys("zhaoliying")
driver.find_element_by_id("password").send_keys("123error")

driver.find_element_by_id("bt-login").click()
time.sleep(1)
result = driver.find_element_by_id("showResult").text
print(result)
```

## 八、下拉选择框、弹出框、滚动条操作

### 1、下拉选择框操作

- 下拉选 择实现步骤：

  1、导入Select类

  2、实例化select对象      select=Select(element)  # <font color=red>element对象表示的是select元素对象</font>

  3、通过select的相关方法选择option选项

  - select.select_by_index(index)    参数index表示的option索引
  - select.select_by_value(value)    参数value表示的是option元素中value的属性值
  - select.select_by_visible_text(visible_text ) 参数visible_text表示的是option的文本内容。

  ```python
  from selenium import webdriver
  from selenium.webdriver.support.select import Select
  import time
  
  # 1.打开浏览器，进入登录页面
  driver = webdriver.Chrome()
  driver.implicitly_wait(10)
  driver.get(url='http://127.0.0.1:7000/dadashop/templates/login.html')
  
  # 2. 登录
  driver.find_element(By.ID, 'username').send_keys('chaogege')
  driver.find_element(By.ID, 'password').send_keys('123456')
  driver.find_element(By.ID, 'bt-login').click()
  time.sleep(1)
  driver.switch_to.alert.accept()
  
  # 3.点击地址管理
  driver.find_element(By.XPATH, '/html/body/header/div/div[3]/ul/li[1]/a').click()
  
  # 4. 选择河北省休眠1秒
  element = driver.find_element(By.CSS_SELECTOR, '#receiverState')
  select = Select(element)
  time.sleep(1)
  select.select_by_index(3)
  # 5.广东省
  time.sleep(1)
  select.select_by_value('广东省')
  # 6.河南省
  time.sleep(1)
  select.select_by_visible_text('河南省')
  
  time.sleep(3)
  driver.quit()
  ```


### 2、弹出框操作

- 弹出框处理步骤：
  - driver.switch_to.alert  获取弹出框对象
  - 处理弹出框    
    - alert.text 获取弹出框提示信息
    - alert.accept()    确定弹出框
    - alert.dismiss()  取消弹出框 

```python
# 登录进入主页
from selenium import webdriver
from selenium.webdriver.common.by import By

driver = webdriver.Chrome()
driver.get(url='http://127.0.0.1:7000/dadashop/templates/login.html')

driver.find_element(By.ID, 'username').send_keys('chaogege')
driver.find_element(By.ID, 'password').send_keys('123456')

driver.switch.alert.accept()
```



### 3、滚动条操作

- 1、定义js

​       js = "window.scrollTo(0, 2000)"  # 如果想要移动到最下方，y值给最大值就可以了。

- 2、执行JS

​       driver.execute_script(js)

```python
from selenium import webdriver
import time

driver = webdriver.Chrome()
driver.get(url='http://127.0.0.1:7000/dadashop/templates/index.html')

driver.excute_script('window.scrollTo(0, 2000)')

time.sleep(3)
driver.quit()
```

------



## 九、frame切换

### 1、frame切换

- frame切换实现方法：
  - driver.switch_to.frame(frame_reference) --> 切换到指定frame的方法
    frame_reference：可以为frame框架的name、id或者定位到的frame元素
  - . driver.switch_to.default_content() --> 恢复默认页面方法

frame切换原理总结:

- 针对同一层级的frame，如果要进行切换的话，需要切回到默认首页
- 针对所要进入的frame， 有多少个层级，就需要切换几次
- 不管当前在哪个层级，如果要回到默认首页，只需要调用一次回到默认首页的方法(driver.switch_to.default_content())

```python
'''
登录网易云音乐官网，进入排行榜，发表评论
'''

from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.wait import WebDriverWait

driver = webdriver.Chrome()
driver.maximize_window()
driver.implicitly_wait(10)
driver.get(url='https://music.163.com/#')

# 点击登录
driver.find_element(By.CLASS_NAME, 'link').click()

# 点击选择其他登录模式、勾选同意、点击手机号登录、点击密码登录
WebDriverWait(driver, 10, poll_frequency=0.5).until(lambda x:x.find_element(By.XPATH, '//*[@id="otherbtn"]/a')).click()
WebDriverWait(driver, 10, poll_frequency=0.5).until(lambda x:x.find_element(By.XPATH, '//*[@id="j-official-terms"]')).click()
WebDriverWait(driver, 10, poll_frequency=0.5).until(lambda x:x.find_element(By.XPATH, '//div[@class="f-mgt10"]/a')).click()
WebDriverWait(driver, 10, poll_frequency=0.5).until(lambda x:x.find_element(By.XPATH, '//div[@class="captchaloginwrapper"]/div[3]/a')).click()
# 输入手机号 密码
driver.find_element(By.NAME, 'p').send_keys('13603263409')
driver.find_element(By.NAME, 'pw').send_keys('zhanshen001')
driver.find_element(By.CLASS_NAME, 'j-primary').click()
# 点击排行榜
driver.find_element(By.XPATH, '//*[@id="g_nav2"]/div/ul/li[2]/a').click()
# 切换切换iframe
driver.switch_to.frame('contentFrame')

# 找到文本框，发表评论
driver.find_element(By.CLASS_NAME, 'u-txt').send_keys('确实不错')
driver.find_element(By.CLASS_NAME, 'btn').click()

# 休眠10秒后关闭浏览器
time.sleep(10)
driver.quit()
```

------



## 十、窗口截图、验证码处理

### 1、窗口截图

- 目的：当程序运行出错时，通过截图文件能够更直观的知道具体是哪个操作步骤出错

- 窗口截图就是指把出错的图片保存到指定位置。

  载图方法：  driver.get_screenshot_as_file(filename)

  - 截图的文件名必须是以PNG结尾
  - filename中的文件目录必须手动创建

------

### 2、验证码处理

#### 2.1 什么是验证码？

验证码就是指一种随机生成的信息（数字、字母、汉字、图片、算术题）等为了防止恶意的请求行为，增加应用的安全性。

自动化过程中也是需要进行注册或者登录的操作，所以需要处理验证码。

#### 2.2 验证码处理方式

- 测试环境去掉验证码
- 测试环境设置万能验证码
- 验证码识别技术     由于技术难度高，识别率很难达到100%， 一般不建议使用







