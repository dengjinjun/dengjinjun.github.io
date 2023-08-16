---
title: 前端开发-HTML
date: 2023-08-15 22:39:36
tags: python
---

# 前端开发-HTML

```python
-前端开发：HTML、CSS、JavaScript
-web框架：接收请求并处理
-MySQL数据库：存储数据地方
```

## 快速搭建网站

一个简单的网页

```python
from flask import Flask

app = Flask(__name__)

@app.route("/show/info")
def index():
    return "常州大学"

if __name__ == '__main__':
    app.run()
```

服务器返还给用户的是字符串格式的带有HTML标签的文本，浏览器能够识别这种文本

Flask框架支持将这种文本写入到文件中

```python
from flask import Flask,render_template

app = Flask(__name__)

@app.route("/show/info")
def index():
    return render_template("文件名")
# 文件名需要在templates文件下，是HTML文件

if __name__ == '__main__':
    app.run()
```

## 浏览器能识别的标签

### 展示信息相关的标签

- 编码

  ```html
  <meta charset="UTF-8">
  ```

- title

  ```html
  <title>常州大学</title>
  ```

- h

  ```html
  标题，块级标签
  <h1>一级标题</h1>
  <h2>二级标题</h2>
  <h3>三级标题</h3>
  ```

- div

  ```html
  div是块级标签，独占一整行
  ```

- span

  ```html
  span是行内标签，内联标签，内容有多大就占多少空间
  ```

- a

  ```html
  超链接，行内标签
  <a href="url">点击这里跳转页面</a>
  这里的url可以是外部网页，也可以是本项目中的网页(相对路径)，默认是从当前页面打开
  <a href="url" target="_blank">点击这里跳转页面</a>
  这里默认是从新页面打开
  a标签默认有下划线，通过text-decoration:none 可以去除
  ```

- img

  ```html
  自闭合标签，行内标签
  <img src="图片地址" />
  这里的图片地址可以是外部网页的图片，存在无法显示的风险(防盗链)
  也可以是本项目中的图片
  	--首先项目中建立static目录，将图片放到static目录中
  	--src="/static/图片名"
  ```

  ```html
  设置图片的高度和宽度,仅设置高度或宽度会进行原比例放缩
  <img style="height:100px;width:50px" src="/static/11.jpg"/>
  px是指像素，绝对大小
  20%是指原大小的20%，相对大小
  ```

- 列表

  ```html
  块级标签
  无序列表
  <ul>
      <li>武进校区</li>
      <li>西太湖校区</li>
      <li>白云校区</li>
  </ul>
  有序列表
  <ol>
      <li>武进校区</li>
      <li>西太湖校区</li>
      <li>白云校区</li>
  </ol>
  ```

- 表格

  ```html
  <thead>是表头，</tbody>是表内容，<tr>代表一行，<th>是表头单元，<td>是表内容单元
  <table border="1">	
      <thead>
      <tr>    <th>id</th>	 <th>姓名</th>   <th>年龄</th> </tr>
      </thead>
      <tbody>
      <tr>    <td>1</td>   <td>张三</td>   <td>16</td>   </tr>
      <tr>    <td>2</td>   <td>李四</td>   <td>16</td>   </tr>
      <tr>    <td>3</td>   <td>王五</td>   <td>16</td>   </tr>
      </tbody>
  </table>
  ```

### 提交数据相关的标签

- input(行内标签)

  - 文本框

    ```html
    <input type="text">
    ```

  - 密码框

    ```html
    <input type="password">
    ```

  - 选择文件

    ```html
    <input type="file">
    ```

  - 单选框

    ```html
    两个单选框的name属性相同，可以保证不能同时选中
    <input type="radio" name="xx">男
    <input type="radio" name="xx">女
    ```

  - 复选框

    ```html
    <input type="checkbox">唱
    <input type="checkbox">跳
    <input type="checkbox">rap
    <input type="checkbox">篮球
    ```

  - 按钮

    ```html
    普通的按钮
    <input type="button" value="提交">
    ```

    ```html
    提交表单
    <input type="submit" value="提交">
    ```

  - 下拉框

    ```html
    默认是单选下拉框，<select multiple>是多选下拉框
    <select>
        <option>北京</option>
        <option>上海</option>
        <option>广州</option>
        <option>深圳</option>
    </select>
    ```

  - 多行文本框

    ```html
    <textarea>可以页内拉宽区域的文本框</textarea>
    ```

  - 表单

    ```html
    表单里的数据可以被submit按钮提交，提交方式有Get和Post两种
    <form method="get" action="提交的地址">
        <input type="submit" value="提交">
    </form>
    ```

    ```html
    页面上的数据想要提交到后台
    	<form>标签包裹要提交的数据
        <form>标签表明提交的方式method="post"和提交的地址action="/xx/xx"
        <form>标签中存在submit按钮
        <form>中的其他标签必须具有name属性
    
    ```

    