[TOC]

## 一、AJAX

### 1）定义

​	浏览器异步的技术，通过JS异步向服务器发送请求并获取到响应数据。

### 2）特点

​	浏览器异步访问，可以做到局部刷新，提升用户体验。典型应用场景：局部刷新、搜索建议、前后端分离。

## 二、异步对象XMLHttpRequest

### 1）XHR定义

​	XHR是Ajax的核心对象，叫做异步对象，代替浏览器向服务器发送异步请求并获取响应。

### 2）使用流程（<font color=red>了解</font>）

1. 创建异步对象
2. 创建请求
3. 发送请求
4. 接收响应

* 示例

  ```python
  var xhr = XMLHttpRequest();
  xhr.open("请求方法", "请求URL地址");
  xhr.send("请求体数据,GET请求填写NULL");
  xhr.responseText
  ```

## 三、跨域

### 1）什么是跨域

​	非同源的网页相互发送请求的过程，叫做跨域。

### 2）浏览器同源策略

* 同源定义：`相同协议、相同域名、相同端口`
* 同源策略：`在HTTP中，必须为同源地址才可以相互发送请求，非同源地址的请求会被浏览器拒绝（浏览器控制台出现：同源策略禁止读取位于......，则一定是跨域的问题）。`

## 四、JQuery对Ajax支持

### 1）`$.get() $.post()`

<font color=red>发送GET和POST请求。</font>

* $.get(url, data, callback, type)

* $.post(url, data, callback, type)

* 示例

  ```python
  $("#btn1").on("click", function(){
      $.get(
      	"url地址",
          {"username": "xxx"},
          function(resp){对响应对应的操作}
      )
  })
  ```

### 2）$.ajax()

<font color=red>发送所有方法的请求。</font>

* $.ajax({参数对象})

* 参数对象属性

  ```python
  $.ajax({
      type: 请求方法,
      url: 请求路由,
      data: {} 查询字符串或者请求体数据,
      success: function(resp){} 响应成功后的回调函数,
      error: function(xhr){} 错误的回调函数,
      beforesend: 发送ajax异步请求之前执行的操作,比如发送之前可以设置请求头,
  })
  ```






















