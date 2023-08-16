---
title: 前端开发-CSS
date: 2023-08-15 22:39:36
tags: python
---

# 前端开发-CSS

CSS，专门用来美化标签

- 基础CSS，写简单页面，能够看懂别人的样式，能够理解并修改
- 基于模板进行调整和修改

## 快速了解CSS

### CSS应用方式

- 直接在标签上应用

  ```html
  <img src="..." style="height:100px"/>
  <div style="color:red;"></div>
  ```

- 在<head>标签中写<style>标签

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>用户登录</title>
      <style>
          .c1{
              color:res;
          }
      </style>
  </head>
  <body>
  <h1 class="c1">标题</h1>    
  </body>
  </html>
  ```

- 链接到文件中

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>用户登录</title>
  	<link rel="stylesheet" href="xx.css"/>
  </head>
  <body>
  <h1 class="c1">标题</h1>    
  </body>
  </html>
  ```

  

用Flask框架开发不方便的地方

- 每次都需要重启
- 规定有些文件必须要放到特定的文件夹
- 新创建一个页面
  - 编写函数
  - 新建HTML文件

### CSS选择器

- 类选择器

  ```html
  .c1{
  	...
  }
  ```

- id选择器(不重复)

  ```html
  #c1{
  	...
  }
  ```

- 标签选择器（过于绝对）

  ```html
  div{
  	...
  }
  li{
  	...
  }
  ```

- 属性选择器

  ```html
  可以与上述三种选择器结合使用，比如
  .c1[value="1"]{
  	...
  }
  input[type="text"]{
  	...
  }
  ```

- 后代选择器

  ```html
  类选择器和标签选择器可以搭配使用
  .c1 li{
  	...
  }
  类为c1的标签，其子孙li标签会应用这种样式
  
  .c1>li{
  	...
  }
  类为c1的标签，只有其子代li标签会应用这种样式
  ```

### 多个样式和覆盖的问题

```html
<style>
    .c1{
        ...
    }
    .c2{
        ...
    }
</style>
<div class="c1 c2">cczu</div>   
<div class="c2 c1">cczu</div> 		
# 无论class属性是先定义哪个类，由于在样式中是c2后定义的，所以c2会覆盖c1
```

```html
<style>
    .c1{
        ...	!important		# 这样就不会被覆盖
    }
    .c2{
        ...
    }
</style>
<div class="c1 c2">cczu</div>   
<div class="c2 c1">cczu</div> 
```

## 基本CSS样式

- 高度和宽度

  ```html
  .c1{
  	height:50px;
  	width:50px;
  }
  高度不支持百分比，宽度支持百分比，比如：width：30%
  对于块级标签，设置了空间之后，剩余空间默认无法占用
  行内标签默认不遵守高宽设置和边距设置，而是遵守内容本身的空间设置
  ```

- 行内、块级特性

  ```html
  display:inline	# 使块级标签转化为行内标签
  display:block	# 使行内标签转化为块级标签
  display:inline-block;	# 即具有行内标签的特性，又具有块级标签的特性
  ```

- 字体设置

  ```html
  color:#00ff00;		# 字体颜色
  font-size:30px		# 字体大小
  font-weight:600		# 字体加粗
  font-family:Microsoft Yahei		# 字体类型（微软雅黑）
  ```

- 文字对齐方式

  ```html
  text-align:center;		# 水平方向居中
  line-height:??px		# ??指行高，需要与整块的行高相同，则文字自动垂直居中
  ```

- 浮动

  ```html
  默认文本在左边
  float:right		# 右边
  float属性可以消除块级标签占据整行的特性，也会使标签脱离文档流
  解决脱离文档流的问题，需要增加一个标签中设置clear:both
  ```

- 边距

  ```html
  padding-top:20px	内边距，与自身内部内容之间的距离，right、bottom、left
  padding:20px		上下左右的内边距都是20px
  padding:20px 10px 5px 5px		上、右、下、左
  ```

  ```html
  margin-top:20px		外边距，与外部内容的距离
  margin:0 auto		上边距为0，左右自动（居中）
  ```

- 透明度

  ```html
  opacity:0.5
  透明度从0到1
  ```

  - 幕布

  ```html
  .mask{
  	background-color:black;
  	position:fixed;
  	left:0;
  	right:0;
  	top:0;
  	bottom:0;
  	opacity:0.7;
  	z-index:999;	# 用于设置显示层先后的
  }
  ```

  

- 鼠标放到标签上之后的样式

  ```html
  .c1:hover{		以上的选择器后加上:hover即可
  	...
  }
  ```

- 标签后追加

  ```html
  .c1:after{
  	...
  }
  ```

  - 一个重要的应用，解决float之后无法撑起父级块的问题

    ```html
    <style>
        .clearfix:after{
            content:"";
            display:block;
            clear:both;    
        }
        .item{
            float:left
        }
    </style>
    <body>
        <div class="clearfix">
            <div class="item">1</div>
            <div class="item">2</div>
            <div class="item">3</div>
        </div>
    </body>
    ```

- position

  - fixed

    ```html
    距离页面顶部和两端的固定位置
    .c1{
    	position:fixed;
    	height:200px;
    	width:200px;
    	left:50px;
    	top:50px;
    }
    设置为居中
    .c1{
    	position:fixed;
    	height:200px;
    	width:200px;
    	left:0;
    	right:0;
    	margin:0 auto;
    	top:50px;	
    }
    ```

  - relative和absolute

    ```html
    relative和absolute配合使用，属性为absolute的标签相对于属性为relative的标签改变位置
    .c1{
     	height:500px;
    	width:300px;
    	position:relative
     }
    .c1 .c2{
    	height:59px;
    	width:59px;
    	position:absolute;
    	top:30px;
    	right:30px;
    }
    ```

- 边框

  ```html
  border:3px solid red		全部边框：粗细，样式，颜色
  border-left:3px solid red	左边框
  透明色：transparent
  ```

- 背景色

  ```html
  background-color:red		
  background-color:#00ff00	RGB颜色
  ```

  