---
title: 初识Django
date: 2023-08-15 22:39:36
tags: python
---

# 初识Django

##  安装Django

```
pip install django
```

安装好Django模块之后，python目录下会增加几个文件

```
Python
	-python.exe
	-Scripts
		-pip.exe
		-django-admin.exe		工具，用于创建Django项目中的文件和文件夹
	-Lib
		-内置模块
		-site-packages
			-openpyxl
			-flask
			-django				框架的源码
```

## 创建项目

Django项目中会有一些默认的文件和文件夹,需要利用工具去生成

- 在终端创建

  - 打开终端

  - 进入项目想要存放的目录

  - 执行命令创建

    ```
    "django-admin.exe的文件地址" startproject 新建项目的名称
    ```

- 使用pycharm创建

  存在一些问题，需要修改

## 认识项目

### 默认项目中的文件介绍

```
mysite
	-manage.py				【项目的管理、启动项目、创建app、数据管理】(经常用到，需要修改)
	-mysite
		-__init__.py
		-settings.py		【项目配置】(常常修改)
		-urls.py			【建立url和函数的对应关系】(常常修改)
		-asgi.py			【接收网络请求】(不修改)
		-wsgi.py			【接收网络请求】(不修改)
```

### APP

APP是Django项目中的一个独立结构，可以分管不同的功能，开发大型Django项目时，需要创建多个独立APP。

- 创建APP

  ```
  python manage.py startapp app01
  ```

- 认识项目目录

  ```
  ├─app01
  │  │  admin.py				【不用动】django默认提供了admin后台管理
  │  │  apps.py				【不用动】app启动类
  │  │  models.py				【重要】对数据库进行操作
  │  │  tests.py				【不用动】单元测试
  │  │  views.py				【重要】函数相关
  │  │  __init__.py
  │  └─migrations				【不用动】数据库变更记录
            __init__.py
  
  
  ```

- 注册APP

  ```
  需要在项目的settings.py文件中INSTALLED_APPS变量列表中写入app01.apps.App01Config
  ```

- 编写url和视图函数的对应关系

  ```
  需要在项目的urls.py文件中的urlpatterns变量列表中写入url和视图函数的对应关系（确保views已导入）
  ```

- 编写视图函数

  ```
  将函数写入views.py文件中，注意函数存在默认参数request
  ```

### 启动Django项目

- 命令行启动

  ```
  python manage.py runserver
  ```

- pycharm启动

### templates目录

Django项目中的HTML模板文件需要放在app目录下的templates目录中，这样才能被函数找到

```
return render(request, "index.html")
```

### static目录

Django项目中的静态文件（img，css, js, plugins）需要放在app目录下的static目录中

HTML页面引入静态文件时，需要在文件头写上

```
{% load static %}
```

然后在具体引入位置的url中

```
{% static 'url地址' %}
```

# 模板语法

本质是在HTML文件中写一些占位符，由数据对这些占位符进行替换和处理

```
n1 = "张三"
n2 = ["张三", "李四", "王五"]
n3 = {"id":1, "name":"邓胖胖", "pwd":"111"}
```

- 单个变量

  ```html
  <div>{{ n1 }}</div>			"张三"
  <div>{{ n2 }}</div>			["张三", "李四", "王五"]
  <div>{{ n2.0 }}</div>		"张三"
  <div>{{ id }}</div>			1
  <div>{{ n3.name }}</div>	"邓胖胖"
  ```

- 循环

  ```html
  {% for item in n2 %}
  	<span>{{item}}</span>
  {{% endfor %}}				张三 李四 王五
  ```

  ```html
  <ul>
      {% for k,v in n3 %}
      <li>{{ k }},{{ v }}</li>
      {% endfor %}
  </ul>
  id,1
  name,邓胖胖
  pwd,111
  ```

- 条件语句

  ```html
  {% if n1 == "张三" %}
  	<h1>1</h1>
  {% elif n1 == "李四" %}
  	<h1>2</h1>
  {% else %}
  	<h1>3</h1>
  {% endif %}					1
  ```

  注意：

  >模板语法是Django框架提供给python的编程规则，浏览器并不能解析含有模板语法的HTML文件。Django中视图函数的render内部会先读取到含有模板语法的HTML文件，在内部进行渲染（执行模板语法并替换数据），最终得到只含有HTML标签的字符串，再返还给浏览器。
  
  ### 注意
  
  > HTML文件中使用python语句要用到模板，但是模板语法跟正常python语法有一些不同, 比如：
  >
  > - 不能出现`()`,模板语法会自动添加
  > - 时间转换，datetime类型的时间转换为字符串：`t1|date:"Y-m-d H:i:s"`

# 关于网络请求

request是一个对象，封装了用户发送的所有请求相关数据

- 请求

```python
print(request.method)	# 获取请求方式
print(request.GET)		# 获取url上传递的值
print(request.POST)		# 获取请求体中传递的值
```

- 响应

```python
return HttpResponse("返回内容")						# HttpResponse方式返还给请求者一个字符串
return render(request, "index.html", {"title":"标题"})	# render方式返还给请求者一个渲染过的HTML文本
return redirect("https://www.baidu.com")				# 让浏览器重定向到其他页面
```

>Django框架对于获取请求体传递的值时，有一层保护机制
>
>在HTML文件的表单中添加`{% csrf_token %}`可以解决

# 数据库操作（ORM）

python代码通过数据库相关的模块（如pymysql）可以操作数据库，但是在Django框架中显得繁琐，Django框架内部提供了操作数据库的框架：ORM框架。

ORM的作用相当于在python代码和数据库相关的模块（mysqlclient等）之间桥梁，或是充当翻译，可以使沟通更加便捷，从而使代码编写更简单。

ORM可以做两件事：

>创建、修改、删除数据库中的表。[不能创建数据库]
>
>操作表中的数据

所以使用ORM时，必须要有提前创建好的数据库。

## 安装和连接

- 安装数据库模块

  ```
  pip install mysqlclient
  ```

- Django连接MySQL

  在settings.py文件中配置和修改

  ```python
  DATABASES ={
  	'default': {					# 默认连接的数据库
  		'ENGINE': 'django.db.backends.mysql',
          'NAME': 'db_1', 			# 数据库名字
  		'USER': 'root',				# 用户名
  		'PASSWORD': 'root123',		# 密码
  		'HOST': '127.0.0.1', 		# 哪台机器安装了MySQL
  		'PORT': 3306,				# 端口号
      }
  }
  ```

## 操作数据表

### 操作表结构

>首先，需要在models.py文件中进行操作
>
>其次，需要在终端执行更新命令

```
python manage.py makemigrations
python manage.py migrate
```

- 在models.py中添加类

```python
class UserInfo(models.Model):
	name = models.CharField(max_length=32)			# 字段名 = 数据类型(最大长度)
	password = models.CharField(max_length=64)
	age = models.IntegerField()
# 这样可以创建一个数据表，表名为 APP名称_类名的小写，如app01_userinfo
```

- 在终端中输入命令用于更新数据库(app需要提前注册)

>删除表和删除字段时，在models.py文件中把类给注释掉，然后执行终端执行两行命令就可以
>
>添加字段时，要考虑到新增字段的数据问题
>
>- 可以选择默认值
>
>  age = models.IntegerField(default=18)
>
>- 可以允许为空
>
>  age = models.IntegerField(null=True, blank=True)
>
>否则在终端输入命令时，会让再次选择手动输入默认值或者退出

### 连表操作

数据库中的两张表有关联，比如已经存在的用户表`userinfo`，包含`id`，`用户名`，`密码`，`年龄`四个字段。

另要新建一张关注表，要求其中一个字段`关注人`,必须是用户表中的用户，可以与用户表的`id`相关联

```python
concern_er=models.ForeignKey(to='userinfo', to_filed='id', on_delete=models.CASCADE)
# to代表与哪张表关联,to_filed代表与哪个字段相关联，models.CASCADE代表级联删除
```

也可以不采用级联删除的方式，若用户被删除，则与其相关的这条字段置空

```python
concern_er=models.ForeignKey(to='userinfo', to_filed='id',null=True,blank=True, on_delete=models.SET_NULL)
```



## 操作表中数据

>注意：在函数中操作表中数据时，需要先导入类 

- 新增数据

  ```
  表名.objects.create(字段名=值,字段名=值,字段名=值)
  ```

  ```python
  # 比如：
  userinfo.objects.create(name="张三", password="123",age=18)
  ```

- 删除数据

  ```python
  表名.objects.filter(条件).delete()		# 删除指定数据，条件一般是id
  表名.objects.all().delete()			# 删除全部数据
  ```

  ```python
  # 比如：
  userinfo.object.filter(id=3).delete()
  ```

- 查询数据

  ```python
  data_list = 表名.objects.filter(条件)
  data_list = 表名.objects.all()
  # 得到的是一个Queryset类型的对象，形式类似于列表，列表中也是多个对象，封装了每一行的数据
  # 如果加上.exists()就可以获得一个布尔值，存在为True，不存在为False
  # 如果加上.count()就可以获得数字，搜索出来的总条目数
  ```

  ```python
  # data_list里面封装的对象可以通过循环获得，比如：
  for obj in data_list:
      print(obj.id, obj.name, obj.age)
     
  # 如果加上.first()就可以获得列表里的第一个对象
  row_obj = 表名.objects.filter(条件).first()
  print(row_obj.name, row_obj.age)
  ```

  - 排序

    ```
    表名.objects.all().order_by("字段名")	# 正向排序，从小到大
    表名.objects.all().order_by("-字段名")	# 逆向排序
    ```

    

- 修改数据

  ```python
  表名.objects.all().update(字段名=值)
  表名.objects.filter(条件).update(字段名=值)
  ```

  ```python
  # 比如
  userinfo.object.filter(name="张三").update(age=10)
  ```

  

# 模板的继承

- 定义母版

  ```html
  <!--母版的文件名：xx.html-->
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>发帖页面</title>
  </head>
  <body>
  	{% block content %}	{% endblock %}
  </body>
  </html>
  ```

- 继承母版

  ```html
  {% extend 'xx.html' %}
  	{% block content %}
  	<h1>写入自定义内容</h1>
  	{% endblock %}
  ```

  