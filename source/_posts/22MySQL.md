---
title: MySQL
date: 2023-08-15 22:39:36
tags: python
---

# MySQL

web框架的作用：

- 找到HTML文件，读取所有的内容
- 找到内容中的特殊的占位符，将数据替换
- 将替换后的内容返还给用户的浏览器

动态的数据需要存储到一个地方：

- txt文件

- excel文件

- 数据库管理系统:解析指令，对文件和文件夹进行操作

  ```
  MySQL(免费版)
  Oracle/SQLServer/DB2/Access...
  ```


## 初识MySQL

### MySQL的安装和配置

```
5.7.31
初始化："D:\mysql\mysql-5.7.31-winx64\bin\mysqld.exe"  --initialize-insecure
```

### MySQL的关闭和启动

- 临时启动

  ```
  D:\mysql\mysql-5.7.31-winx64\bin\mysqld.exe
  ```

- 制作成windows服务

  ```
  "D:\mysql\mysql-5.7.31-winx64\bin\mysqld.exe"  --install mysql57
  ```

  - 启动和关闭

    ```
    net start mysql57
    net stop mysql57
    ```

- 任务管理器-服务-mysql57

### 连接测试

三种方式连接MySQL服务

- MySQL自带服务

  ```python
  >>>"D:\mysql\mysql-5.7.31-winx64\bin\mysql.exe"  -h 127.0.0.1 -P 3306 -u root -p
  # -h 后是服务器地址；-P 代表端口；-u是用户；-p是密码
  ```

  ```python
  >>>"D:\mysql\mysql-5.7.31-winx64\bin\mysql.exe" -u root -p
  # 不写-h和-P默认连接本地
  ```

  将mysql.exe的地址添加到环境变量之后

  ```python
  >>>mysql.exe -u root -p
  ```

  - 修改密码

    ```
    set password = password('123456')
    ```

  - 查看已有数据库

    ```
    show databases;
    ```

  - 关闭连接

    ```
    exit;
    ```

- 第三方工具
- 高级语言代码

### 忘记密码的解决办法

- 关闭MySQL服务

- 修改MySQL配置文件（my.ini）,文件中加上

  ```
  skip-grant-tables=1
  ```

- 重新启动MySQL，此时是无账号模式，可以直接登录

- 输入指令

  ```
  use mysql;
  ```

  ```
  update user set authentication_string = password('新密码'),password_last_changed=now() where user='root';
  ```

- 关闭MySQL服务，修改配置文件，把最后一句删除

- 启动MySQL，使用新密码登录

## MySQL基本指令

### 数据库管理

- 查看当前已有的数据库

  ```
  show databases;
  ```

- 创建数据库

  ```
  create database 数据库名字 DEFAULT CHARSET utf8 COLLATE utf8_general_ci;
  ```

- 删除数据库

  ```
  drop database 数据库名字;
  ```

- 进入数据库

  ```
  use 数据库名;
  ```

- 查看数据库下所有数据表

  ```
  show tables;
  ```

### 数据表管理

- 创建数据表

  ```
  create table 表名称(
  	列名称	类型,
  	列名称	类型,
  	列名称	类型
  )default charset=utf8;
  ```

  - 主键(不允许为空，也不允许重复)

    ```
    列名称	类型 primary key,
    ```

  - 自增的主键

    ```
    列名称	类型 not null auto_increament primary key,		
    ```

  - 非空(不设置默认可以为空)

    ```
    列名称	类型 not null,
    ```

  - 默认值

    ```
    列名称	类型 default 默认值,
    ```

- 删除表

  ```
  drop table 表名称;
  ```

- 展示表结构

  ```
  desc 表名;
  ```

  ### 数据行操作

- 向表中插入数据

  - 添加单个数据

  ```
  insert into 表名(列名) values(值);
  ```

  - 批量添加

  ```
  insert into 表名(列名) values(值),(值),(值),(值);
  ```

  - 多列添加

  ```
  insert into 表名(列名,列名,列名) values(值,值,值),(值,值,值),(值,值,值);
  ```

- 删除数据

  - 删除所有数据

  ```
  delete from 表名;
  ```

  - 按条件删除

  ```
  delete from 表名 where 条件;
  ```

- 修改数据

  - 修改全列

  ```
  update 表名 set 列名=值;
  ```

  - 批量修改

  ```
  update 表名 set 列名=值,列名=值;
  ```

  - 按条件修改

  ```
  update 表名 set 列名=值 where 条件;
  ```

  - 对于值的修改可以更灵活，比如不必是固定值，而是在原来的基础上变化，比如：

  ```
  update tb1 set age=age+10 where id>5;
  ```

- 查询数据

  - 查询表中全部数据

  ```
  select * from 表名;
  ```

  - 按部分列的查询

  ```
  select 列名,列名 from 表名;
  ```

  - 按条件查询

  ```
  select 列名,列名 from 表名 where 条件;
  ```

  

## MySQL数据类型

- tinyint

  ```
  tinyint是整型的数字，有两种形式：
  有符号，可以表示-128~127范围的整数(默认状态)，比如：age tinyint
  无符号，也可以表示0~255范围的整数(需要定义)：age tinyint unsigned
  ```

- int

  ```
  与tinyint类似，也分为有符号和无符号两种：
  int 			有符号，取值范围-2147483648~2147483647
  int unsigned	无符号，取值范围0~4294967295
  ```

- bigint

  ```
  同样分为有符号和无符号，范围更大，不再赘述
  ```

- float(小数，不精准)

- double(小数，不精准)

- decimal(精准数)

  ```sql
  例如添加薪资字段：
  salary decimal(8,2)		表示小数一共有8位，小数点后有两位，小数点前有6位
  decimal虽然精准，但是存储的小数小数点前最多65位，小数点后最多30位，支持四舍五入
  ```

- char

  ```
  定长字符串,最多255个字符，比如：
  列名 char(11)，	固定用11个字符进行存储，即使不到11个字符，也按照11个字符存储，存在浪费空间的情况
  ```

- varchar

  ```
  变长字符串，最多65535(2**16-1)个字节，比如：
  列名 varchar(11),		真实数据有多长就按照多长存储，不超过11个
  ```

- text

  ```
  一般用于保存变长的大字符串，最多65535个字符，比如文章，新闻等
  ```

- mediumtext(超长文本，2**24-1)

- longtext(超长文本，2**32-1)

- datetime

  ```
  时间：年月日时分秒
  YYYY-MM-DD HH:MM:SS(2023-03-03 12:23:45)
  ```

- date

  ```
  时间：年月日
  YYYY-MM-DD(2023-03-03)
  ```

  

## python配合MySQL使用

### 第三方模块安装与测试

```
pip install pymysql
```

发送指令并获取MySQL返回的结果

```python
import pymysql


# 1.连接mysql
conn = pymysql.connect(host="127.0.0.1", port=3306, user="root", password="123456", charset="utf8", db="数据库名")   # 用于连接
cursor = conn.cursor(cursor=pymysql.cursors.DictCursor)     # 用于发送指令，收发数据

# 2. 发送指令
cursor.execute("sql语句")     # 生成指令
conn.commit()               # 提交指令

# 3.关闭
cursor.close()
conn.close()
```

### 新建数据

不能用字符串格式化做sql语句的拼接，因为会有安全隐患（sql注入）

```python
# 示例： 发送指令
cursor.execute("insert into tb1(username,password) values("张三", "111")")     # 生成指令
conn.commit()               # 提交指令
```

```python
# 发送指令
sql = "insert into tb1(username,password) values("张三", "111")"
cursor.execute(sql)    
conn.commit()
```

```python
# 发送指令
sql = "insert into tb1(username,password) values(%s, %s)"	# %s代表一个占位符
cursor.execute(sql,["张三", "111"])    					# 用列表传值
conn.commit()
```

```python
# 发送指令
sql = "insert into tb1(username,password) values(%(n1)s, %(n2)s)"	# n1,n2代表变量名
cursor.execute(sql,{"n1":"张三", "n2":"111"})    			# 用字典传值
conn.commit()
```

### 删除数据

```python
# 发送指令
cursor.execute("sql语句")    					
conn.commit()
```

### 修改数据

```python
# 示例：发送指令
cursor.execute("update tb1 set password = %s where username = %s", ["222", "张三"])    	
conn.commit()
```

### 查询数据

```python
# 发送指令
cursor.execute("sql语句")			# 生成指令
cursor.fetchall()				# 获取数据
```

```python
# 示例： 发送指令
cursor.execute("select * from tb1")			# 生成指令
data_list= cursor.fetchall()				# 获取所有数据,这里需要有变量接收数据
# data_dict = cursor.fetchone()				这里是仅过去符合条件的第一条数据,若没有数据，返回None
for item in data_list:				# 此处的data_list是列表嵌套字典的数据类型,若没有数据返回空列表
    print(item)								# 输出每个字典
```

注意：

- 新增、删除、修改时，都需要加上`conn.commit`,不然数据库里不更新数据
- 查询时不需要加上`conn.commit`,需要`cursor.fetchall`或`cursor.fetchone`





