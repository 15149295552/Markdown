[toc]

## 一、Ajax

### 1）定义

​	浏览器异步的技术，通过JavaScript向服务器发送请求并接收响应的数据。

### 2）特点

​	浏览器异步访问，可以做到页面局部刷新，提升用户体验。典型的应用场景：搜索建议、前后端分离。

## 二、异步对象XMLHttpRequest

### 1）XHR定义

​	XHR是Ajax的核心对象，叫异步对象，代替浏览器向服务器发送异步的请求并接收响应。

### 2）使用流程（<font color=red>了解</font>）

1. 创建XHR异步对象
2. 创建请求
3. 发送请求
4. 接收响应

* 示例

  ```python
  var xhr = new XMLHttpRequest();
  xhr.open("请求方法", "请求地址", true)
  xhr.send("请求体数据,GET请求设置为null")
  xhr.responseText
  ```

## 三、跨域

### 1）什么是跨域

​	非同源的网页相互发送请求的过程，叫做跨域。

### 2）浏览器同源策略

* 同源定义

  协议相同、域名相同、端口相同的为同源地址。

* 同源策略

  在HTTP中，必须同源的地址才能相互发送请求，非同源的地址的请求会被浏览器拒绝。

## 四、jquery对ajax的支持

### 1）GET/POST请求

* $.get(url, data, callback, type)

* $.post(url, data, callback, type)

* 示例

  ```python
  $("#btn1").on("click", function(){
      $.post(
      	"路由",
          {请求体数据},
          function(resp){
              console.log(resp)
          }
      )
  })
  ```

### 2）所有请求

<font color=red>**GET请求: 查询（在服务器端获取资源）**</font>

<font color=black>**POST请求: 新增（在服务器端新增资源）**</font>

<font color=purple>**PUT请求: 修改（在服务器端更新资源）**</font>

<font color=green>**DELETE请求: 删除（在服务器端删除资源）**</font>

* $.ajax({JS对象})

* 参数对象属性

  ```python
  {
      url: 异步请求地址,
      type: 请求方法, "get" "post" "put" "delete"
      data: 请求体数据,
      dataType: 响应数据格式,
      succes: 回调函数,成功时响应回来需要执行的操作,
      error: 回调函数,失败时需要执行的操作,
      beforeSend: 发送异步请求之前需要执行的操作,
      async: 是否启用异步请求
  }
  ```

  

















