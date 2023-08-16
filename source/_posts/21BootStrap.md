---
title: BootStrap
date: 2023-08-15 22:39:36
tags: python
---

## BootStrap

Bootstrap是别人已经写好的CSS样式，我们使用这个CSS样式，需要

- 下载Bootstrap

  ```html
  https://v3.bootcss.com/
  ```

- 在页面上引用Bootstrap

- 编写HTML时，按照Bootstrap的规定来编写+自定义需求

## 导航条

## 栅格

```html
<div class="col-sm-9">左边占9格</div>
<div class="col-sm-3">右边占3格</div>
```

## container

```html
<div class="container">宽度1175px居中的区域</div>
<div class="container-fluid">铺满全屏的区域</div>
```

## 媒体面板

## 分页

## 图标

- bootstrap提供了一部分图标，但是无法满足多样化需求

- 另一个组件fontawesome

  ```html
  fontawesome.dashgame.com
  ```

# Javascript

- JavaScript是一门编程语言，嵌套在HTML中，浏览器就是JavaScript语言的解释器。

- DOM和BOM，相当于JavaScript语言内置的模块
- jQuery，相当于是JavaScript的第三方模块

## 初识JavaScript

```html
让程序实现动态的效果
<body>

<div onclick="myFunc()">文本</div>

<script type="text/javascript">
	function myFunc{
        confirm("你好，这是弹窗")
    }
</script>
    
</body>    
```

一般页面中的js代码会放在<body>标签的尾部，也可以放在<head>标签中（不推荐）

JavaScript代码的存在形式

- 在当前页面中

```html
<script type="text/javascript">
    定义一些函数
</script>
```

- 在其他页面中，导入使用

  一般也是放在<body>标签尾部，除非需要先执行js部分的代码

```html
<script src="js文件路径"></script>
```

## 注释

- JavaScript注释

  ```javascript
  // 注释内容，放在<script>代码块中 //
  /* 注释内容，放在<script>代码块中 */
  ```

- css注释

  ```css
  /* 注释内容，放在<style>代码块中 */
  ```

- html注释

  ```html
  <!-- 放在HTML文本中 -->
  ```

## 变量

### 定义变量

```javascript
var name="邓胖胖"
```

### 打印

```javascript
console.log(name)
```

### 字符串

```javascript
//声明字符串变量
var name = "常州大学"
var name = String("常州大学");

//获取长度
var v1 = name.length;

//索引
var v2 = name[0];	//常，不存在负数索引
var v2 = name.charAt(0)

//去除空白
var v3 = name.trim();

//切片
var v4 = name.substring(0,2)	//常州，前取后不取
```

- 案例：跑马灯字符串

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>跑马灯案例</title>
  </head>
  <body>
  <span Id="txt">常州大学西太湖校区</span>
  <script type="text/javascript">
      function show(){
          var tag = document.getElementById("txt");    //DOM方法获取HTML中的标签对象
          var dataString = tag.innerText;             //获取标签对象中的内部文本
  
          var firstChar = dataString[0];
          var otherString = dataString.substring(1, dataString.length);
          var newText = otherString + firstChar;
          tag.innerText = newText;
      }
      setInterval(show, 1000);    //js中的定时器，每1000ms执行一次show函数
  </script>
  </body>
  </html>
  ```

### 数组（列表）

```javascript
//声明一个数组
var v1 = [11,22,33,44]
var v1 = Array[11,22,33,44]

//通过索引获取值和修改
v2 = v1[0]		// v2 = 11
v1[2] = "张三"	// [11,22,"张三",44]

//尾部追加
v1.push("李四")	//[11,22,33,44,"李四"]

//头部追加
v1.unshift("李四")	//["李四",11,22,33,44]

//任意位置追加
v1.splice(索引位置,0,元素)
v1.splice(1,0,"张三")		// [11,"张三",22,33,44]

//尾部删除
v1.pop()

//头部删除
v1.shift()

//任意位置删除
v1.splice(索引位置,1)
v1.splice(1,1)		//[11,33,44]

//循环
for (var idx in v1){		//这里的idx是索引
    console.log(v1[idx])
}
```

- 案例：动态数组

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>动态数组</title>
  </head>
  <body>
  <ul id="city"></ul>
  <script type="text/javascript">
      var cityList = ["北京","上海","深圳"];
      for (var idx in cityList){
          var text = cityList[idx];
          var tag = document.createElement("li");		//DOM方法创建标签对象
          tag.innerText = text;
          var parentTag = document.getElementById("city");
          parentTag.appendChild(tag);		//添加子标签
      }
  </script>
  </body>
  </html>
  ```

### 对象（字典）

```javascript
//声明对象
info = {
    name:"张三"	//键名可以带引号，也可以不带
    age:18
}

//获取和修改
info.name		//"张三"
info["age"]		//18，两种方法获取
info.name = "李四"	
info["name"] = "李四"		//同样，两种修改方法
delete info["age"]		//删除
```

- 案例：动态表格

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>动态表格</title>
  </head>
  <body>
  <table>
      <thead>
          <tr>
              <th>ID</th>
              <th>姓名</th>
              <th>年龄</th>
          </tr>
      </thead>
      <tbody id="body"></tbody>
  </table>
  <script type="text/javascript">
      var dataList = [
          {id:1, name:"张三", age:18},
          {id:2, name:"李四", age:18},
          {id:3, name:"王五", age:18}
      ];
      for(var idx in dataList){
          var info = dataList[idx];
          var tr = document.createElement("tr");
          for(var key in info){   //key在这个循环里指键名
              var text = info[key];
              var td = document.createElement("td");
              td.innerText = text;
              tr.appendChild(td);
          }
          var bodyTag = document.getElementById("body");
          bodyTag.appendChild(tr);
      }
  </script>
  </body>
  </html>
  ```

  

## 条件语句

```javascript
if(条件){
    ...
}else{
    ...
}
```

```javascript
if(条件){
    ...
}else if(条件){
    ...
}else if(条件){
    ...
}else{
    ...
}
```

## 函数

```javascript
//定义
function 函数名(){
    ...
}
//执行
函数名()
```

## DOM

DOM就是一个模块，可以对HTML中的标签进行操作

### 基本功能

```javascript
//根据Id获取标签
var tag = document.getElementById("xx");

//获取标签中的文本
tag.innerText

//修改标签中的文本
tag.innerText = "hahahahaha";

//创建标签
var tag = document.creatElement("div");

//添加子标签
parentTag.appendChild(tag);
```

### 事件的绑定

```javascript
onclick="函数名()"
```

```js
DOM可以实现很多操作，但是比较繁琐
便捷实现页面上的效果：JQuery/vue.js/react.js
```

# jQuery

jQuery是JavaScript的一个第三方模块（类库）

- 利用jQuery，可以自己开发功能
- 有些现成的工具，比如Bootstrap会依赖jQuery实现动态效果

## 寻找标签

### 直接寻找

- `id`选择器

  ```html
  <h1 id="t1">标题</h1>
  ```

  ```JavaScript
  $("#t1")
  ```

- 类选择器

  ```html
  <h1 class="t1">标题</h1>
  ```

  ```javascript
  $(".t1")
  ```

- 标签选择器

  ```html
  <h1>标题</h1>
  ```

  ```javascript
  $("h1")
  ```

- 层级选择器

  ```javascript
  $(".c1 .c2 a")		//找到c1类下的c2类下的a标签
  ```

- 多选择器

  ```javascript
  $(".c1,#t1,a")		//同时选择多种类型的标签
  ```

- 属性选择器

  ```javascript
  $("input[name='n1']")	//找到name属性为n1的input标签
  ```

### 间接寻找

- 找到同级的兄弟标签

  ```html
  <div>北京</div>
  <div id="c1">上海</div>
  <div>广州</div>
  <div>深圳</div>
  ```

  ```javascript
  $("#c1")			//上海
  $("#c1").prev()		//北京
  $("#c1").next()		//广州
  $("#c1").next().next()	//深圳
  $("#c1").siblings()	//所有的兄弟
  ```

- 找父子

  ```html
  <div id="c1">上海</div>
  ```

  ```javascript
  $("#c1").parent()			//寻找父级标签
  $("#c1").parent().parent()	//寻找父级标签的父级标签
  $("#c1").children()			//找到所有的儿子标签
  $("#c1").children(".p10")	//在所有的儿子标签中找到类为p10的标签
  $("#c1").find(".p10")		//在所有的子孙标签中找到类为p10的标签
  $("#c1").find("div")		//在所有的子孙标签中找到div标签
  ```

## 操作样式

```javascript
addClass()			//添加样式
removeClass()		//移除样式
hasClass()			//具有样式，用于判断
```

## 值的操作

- 静态文本

```html
<div id="c1">内容</div>
```

```javascript
$("#c1").text()			//获取文本内容
$("#c1").text("hahaha")	//修改文本内容
```

- 交互文本

```html
<input type="text" id="c2"/>
```

```javascript
$("#c2").val()			//获取用户输入的值
$("#c2").val("hahaha")	//设置值
```

## 绑定事件

```html
<ul>
    <li>百度</li>
    <li>搜狗</li>
    <li>谷歌</li>
</ul>
```

```javascript
$("li").click(function(){		//点击选择的标签就会触发这个函数
    ...							//这里写函数语句，$(this)代表当前标签	
})
```

```javascript
$(function(){
    ...							//当页面的框架加载完成之后，就自动执行
})
```

