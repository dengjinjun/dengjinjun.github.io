---
title: Django组件
date: 2023-08-15 22:39:36
tags: python
---

# Django组件

Django组件包括Form组件和ModelForm组件

原始方式

```
-页面代码冗余
-获取关联数据操作繁琐
-用户数据没有校验
-缺少错误提示
```

## Form组件

需要在`views.py` 和  `HTML`文件中进行操作

- views.py

  ```python
  # 定义一个类，类里面写上字段
  class MyForm(Form):		# 括号里的Form是固定的
      user = forms.CharField(widget=forms.input)
      upwd = forms.CharField(widget=forms.input)
      email = forms.CharField(widget=forms.input)
  
  # 在函数中会用到
  def user_add(request):
      if request.method == 'GET':
          form = MyForm()		# 此处用到了上文中的类
          return render(request, 'user_add.html', {'form':form})	# 将form传到HTML文件
  ```

- user_add.html

  ```html
  <form method='post'>
      {% for field in form %}		<!--也可以不循环，输入单个字段的input框-->
      	{{ field }}
      {% endfor %}
  </form>				<!--这里就将这些字段的input框直接传入了HTML文件-->
  ```

  

## ModelForm组件

ModelForm组件比Form组件更加方便

- views.py

```python
# 这里可以直接将models.py文件中的类继承过来
class MyForm(forms.ModelForm):	# 括号里的ModelForm是固定的
    xx = forms.CharField(
        label="确认密码"
        widget=forms.PasswordInput)	# 可以自由添加字段
    class Meta:
        model = models.UserInfo	# model = models.py中的类名，即用户表的名字
    	fields = ["name", "age", "password", "xx"]	# 里面是字段名
        
# 后面跟Form组件的操作是一样的
def user_add(request):
    if request.method == 'GET':
        form = MyForm()		# 此处用到了上文中的类
        return render(request, 'user_add.html', {'form':form})	# 将form传到HTML文件
```

```python
# 关于fields,当数据表内字段较多时，可以直接写全部字段
fields = "__all__"
# 若是需要获取某个字段之外的其他字段，可以写
exclude = ["字段名"]
```

- user_add.html

```html
<form method='post'>
    {% for field in form %}		<!--也可以不循环，输入单个字段的input框-->
    	{{ field }}			<!-- 也可以加上字段解释：{{field.label}} ,获取的是verbose-name -->
    {% endfor %}
</form>				<!--这里就将这些字段的input框直接传入了HTML文件-->

<!--输入单个字段的input框-->
<form method='post'>		
	{{form.name.label}}:{{form.name}}
    {{form.age.label}}:{{form.age}}
</form>
```

>当获取到的是一个对象时，可以在models.py的类里面定义一个函数，使其返回字符串, 比如：
>
>```python
>def __str__(self):
>    return self.字段名
>```

>给字段输入框加上样式：views.py文件函数后添加
>
>```python
>def __init__(self, *args, **kwargs):
>    super().__init__(*args, **kwargs)
>    for name,field in self.fields.items():
>        field.widget.attrs = {"class":"form-control", "placeholder":field.label}
>```

### 获取表单数据，添加到数据库

```python
form = MyForm(data=request.POST)
if form.is_valid():					# 数据校验，成功后放在了字典form.cleaned_data里面
    form.save()
    return redirect('xx/xx/')
return render(request, 'xx.html', {"form":form})   #失败后，每个错误信息保存在数组field.errors
```

```python
# 关于form.save,默认保存form中的字段值，即用户输入的字段值，如果想要添加额外的值，可以在前面加上
form.instance.字段名 = 值
```

### 用正则表达式实现检验规则

首先导入包，才能使用正则表达式

```python
from django.core.validators import RegexValidator
```

在ModelForm类中加上

```python
# 比如手机号字段mobile
    mobile = forms.CharField(
        label="手机号",
        validators=[RegexValidator(r'1[3-9]\d{9}$', '手机号格式错误')]
```

## 关于查询规则

```python
# filter也支持字典查询，引入字典前要加上两个**
dict = ['name':'张三', 'age':18]
表名.objects.filter(**dict)
```

```python
# 除了filter，还可以加上排除条件exclude
row_obj = 表名.objects.filter(条件).exclude(条件)
```

### 万能的`__`

- 数字

```python
表名.objects.filter(id=12)			# 查询id=12的条目
表名.objects.filter(id__gt=12)		# 大于12
表名.objects.filter(id__gte=12)		# 大于等于12
表名.objects.filter(id__lt=12)		# 小于12
表名.objects.filter(id__lte=12)		# 小于等于12
```

- 字符串

```python
表名.objects.filter(text="xx")				# 查询字段text的值等于xx的条目
表名.objects.filter(text__startwith="xx")		# 以xx开头
表名.objects.filter(text__endwith="xx")		# 以xx结尾
表名.objects.filter(text__contains="xx")		# 包含xx
```

### 时间控件datetimepicker

一些设置

```javascript
<script type="text/javascript">				//年月日时分
    $('.form_datetime').datetimepicker({		
        //language:  'fr',
        weekStart: 1,
        todayBtn:  1,
		autoclose: 1,
		todayHighlight: 1,
		startView: 2,
		forceParse: 0,
        showMeridian: 1
    });
	$('.form_date').datetimepicker({		//年月日
        language:  'fr',
        weekStart: 1,
        todayBtn:  1,
		autoclose: 1,
		todayHighlight: 1,
		startView: 2,
		minView: 2,
		forceParse: 0
    });
	$('.form_time').datetimepicker({		//时分
        language:  'fr',
        weekStart: 1,
        todayBtn:  1,
		autoclose: 1,
		todayHighlight: 1,
		startView: 1,
		minView: 0,
		maxView: 1,
		forceParse: 0
    });
</script>
```

### 登录

![image-20230308142715954](assets/image-20230308142715954.png)

### Ajax请求

浏览器向网站发送请求时，有URL和表单两种形式，分别为Get和Post请求，特点是页面会刷新。



