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
  create database dashopt default charset utf8;
  ```

- 删除项目中的迁移文件（**/home/tarena/project/dashopt**）

  - 删除user应用下的迁移文件（<font color=red>**删除 dashopt/user/migrations/0001_initial.py**</font>）

- 迁移同步

  ```linux
  python3 manage.py makemigrations
  python3 manage.py migrate
  ```

## 四、项目代码调整

将settings.py中邮件配置的 <font color=red>QQ邮箱和授权码</font> 替换成自己的。

```PYTHON
EMAIL_HOST_USER = '自己的QQ邮箱'
EMAIL_HOST_PASSWORD = '自己的QQ邮箱授权码'
```

## 五、测试

### 1、启动项目

```python
python3 manage.py runserver
```

### 2、访问主页

```python
http://127.0.0.1:7000/dadashop/templates/index.html
```

### 3、注册用户并确认是否发送邮件

```python
http://127.0.0.1:7000/dadashop/templates/register.html
```

### 4、登录用户

```python
http://127.0.0.1:7000/dadashop/templates/login.html
```

