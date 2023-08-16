---
title: Excel和Word操作
date: 2023-08-15 22:39:36
tags: python
---

# Excel和Word操作

什么是对象？

- 内部包含了很多值和功能的“包裹”

- 归类概念，把同一类打包在一起

## Excel操作

使用python操作Excel需要用到第三方模块，比如`openpyxl`：

```python
pip install openpyxl
```

### 读取操作

```python
from openpyxl import load_workbook

# 获取workbook对象
work_book_object = load_workbook("文件路径")

# 读取所有sheet的名字
sheet_name_list = work_book_object.sheetnames

# 获取所有的sheet对象
sheet_object_list = work_book_object.worksheets

# 获取某个sheet对象，两种方式
sheet_object = sheet_object_list[索引值]			# sheet_object_list[0]，就是第一个sheet
sheet_object = work_book_object["sheet名"]

# 获取sheet名
sheet_object.title

# 获取sheet中的某个单元格对象，两种方式
cell_object = sheet_object.cell(2,5)	# 获取第2行第5列的单元格对象
cell_object = sheet_object["E2"]

# 读取单元格的文本
text = cell_object.value

# 读取单元格的坐标位置
cell_object.coordinate		# 如A1,E3等

# 读取sheet中的某一行(sheet中的行数从1开始)
row_list = sheet_object[1]

# 读取sheet中的所有行，常用于for循环
sheet_object.rows

# 从第n行开始读取某一列，通过其他语句实现
for row in sheet_object.iter_rows(min_row = 2, maxrow = 10):
    print(row[0].value)			# 读取第2行到第10行的第一列数据

# 普通单元格是cell对象，被合并的单元格是Mergedcell对象，不含值
# 获取所有被合并的单元格范围
sheet_object.merged_cells

# 获取到sheet中的最高行数或最高列数，int类型
sheet_object.max_row		# 最高行数
sheet_object.max_cloumn		# 最高列数
```

### 写入操作

#### 两种写入方式

- 修改

  ```python
  from openpyxl import load_workbook
  
  # 读取，将所有内容读取到内存，同上节内容
  work_book_object = load_workbook("p1.xlsx")
  sheet_object = work_book_object.worksheets[0]
  cell_object = sheet_object.cell(1,1)
  
  # 修改，都是在内存中修改，比如
  cell_object.value = "几点回家"
  
  # 保存，可以保存到原文件，也可以保存到新文件
  work_book_object.save("p1.xlsx")	# 保存到原文件
  work_book_object.save("p2.xlsx")	# 保存到新文件
  ```

- 新建

  ```python
  from openpyxl.workbook import Workbook
  
  # 读取(空内容)
  work_book_object = Workbook()	# 新建workbook，默认有一个sheet
  sheet_object = work_book_object.worksheets[0]
  cell_object = sheet_object.cell(1,1)
  
  # 在内存中修改操作
  cell_object.value = "该吃饭了"
  
  # 保存
  work_book_object.save("p2.xlsx")
  ```

#### 能够写入什么

```python
from openpyxl.workbook import Workbook

wb = Workbook()

# sheet名
sheet0 = wb.worksheets[0]
sheet0.title = "西太湖"

# 创建sheet
sheet1 = wb.creat_sheet("武进",1)		# 括号里是sheet名和sheet序号（从0开始） 
sheet2 = wb.creat_sheet("白云",2)

# 拷贝sheet
sheet_object = wb.worksheets[0]
new_sheet = wb.copy_worksheet(sheet_object)
new_sheet.title = "备份"

# 删除sheet
del wb["sheet名"]

# 写入单元格的值
sheet0.cell(1,1).value = "学生"
sheet0["A1"].value = "学生"

# 单元格文本居中格式,自动换行 
# 需要导入from openpyxl.styles import Alignment
cell_object = sheet0.cell(1,1)
cell_object.alignment = Alignment(horizontal="center", vertical="center",warp_text=True)

# 实现一行的文本居中，自动换行
text_list = ["张三", "李四", "王五", "赵六", "吴七"] 
for col,text in enumerate(text_list,1):
    cell = sheet0.cell(1,col)
    cell.value = text
    cell.alignment = Alignment(horizontal="center",vertical="center",warp_text=True)
    
# 单元格边框
# 需要导入from openpyxl.styles import Border, Side
cell.border = Border(
	top=Side(style="thin",color="0000FF")
	bottom=Side(style="medium",color="0000FF")
    right=Side(style="thin",color="0000FF")
    left=Side(style="thin",color="0000FF")
)

# 设置字体大小、颜色、样式
# 需要导入from openpyxl.styles import Font
cell.font = Font(size="40", color="FF0000",name="微软雅黑")

# 设置宽高(单元格都是呈行列排布的，所以要从行列的维度来设置)
sheet0.column_dimensions["A"].height = 100	# 第一列宽设为100
sheet0.row_dimensions[1].height = 50		# 第一行高设为50

# 设置背景色
# 需要导入from openpyxl.styles import PatternFill
cell.fill = PatternFill("solid", fgcolor="FF0000")
```

P18 1:17:xx

