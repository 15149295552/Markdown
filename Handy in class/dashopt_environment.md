[toc]

# 快速搞定环境

## 一、配置并重启Nginx

- sudo gedit /etc/nginx/conf.d/dadashop.conf

  ```nginx
  server {
          
          listen 7000 default_server;
          listen [::]:7000 default_server;
          server_name __;
  
          root /var/www/html;
          # Add index.php to the list if you are using PHP
          index index.html index.htm index.nginx-debian.html;
  
          location / {
                  # First attempt to serve request as file, then
                  # as directory, then fall back to displaying a 404.
                  try_files $uri $uri/ =404;
          }
  }
  ```

- 重新启动nginx

  ```python
  sudo /etc/init.d/nginx restart
  ```

## 二、拷贝前端文件

- 拷贝前端文件

  ```python
  1.将前端文件夹client拷贝到用户主目录下 /home/tarena/
  
  2.将client下的子目录拷贝到nginx目录下：/var/www/html
    # 打开终端，路径在主目录下：/home/tarena/
    sudo mkdir /var/www/html/dadashop/
    sudo cp -fr client/.  /var/www/html/dadashop/
  ```

## 三、配置数据库

- 建库

  ```mysql
  # 进入MySQL命令行：mysql -uroot -p123456
  # 1.如果原来有dashopt库,则先删除原来库
  drop database dashopt;
  # 2.创建新库dashopt
  create database dashopt default charset utf8;
  ```

- 删除项目中的迁移文件（**/home/tarena/project/dashopt**）

  - 删除user应用下的迁移文件（<font color=red>**删除 dashopt/users/migrations/0001_initial.py、0002_address.py、0003_weiboprofile.py**</font>）
  - 删除goods应用下的迁移文件（<font color=blue>**dashopt/goods/migrations/0001_initial.py**</font>）

- 迁移同步

  ```linux
  python3 manage.py makemigrations
  python3 manage.py migrate
  ```

## 四、导入商品模块测试数据

`python3 manage.py loaddata goods_data.json`

## 五、项目代码调整

### 1）邮箱配置

将settings.py中邮件配置的 <font color=red>QQ邮箱和授权码</font> 替换成自己的。

```PYTHON
EMAIL_HOST_USER = '自己的QQ邮箱'
EMAIL_HOST_PASSWORD = '自己的QQ邮箱授权码'
```

### 2）容联云短信配置

```python
# utils/smsapi.py
accId = "自己的信息"
accToken = "自己的信息"
appId = "自己的信息"
```

### 3）微博相关配置

```python
CLIENT_ID = "自己的应用信息"
CLIENT_SECRET = "自己的应用信息"
```

## 六、测试

### 1）启动项目

```python
python3 manage.py runserver
```

### 2）访问主页

```python
http://127.0.0.1:7000/dadashop/templates/index.html
```

### 3）注册用户

```python
http://127.0.0.1:7000/dadashop/templates/register_sms.html
```

### 4）登录用户

```python
http://127.0.0.1:7000/dadashop/templates/login.html
```

#### 5）添加收货地址

#### 6）测试首页展示、详情页展示、购物车相关功能、订单确认页相关功能是否正常
