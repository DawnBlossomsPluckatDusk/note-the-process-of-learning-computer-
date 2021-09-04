# python基础学习

## 参考链接

[廖雪峰python](https://www.liaoxuefeng.com/wiki/1016959663602400/)

[菜鸟教程](https://www.runoob.com/python3/python3-tutorial.html)

## 基础语法

### 编码

默认为`utf-8`编码，可以在文件开头声明使用的编码方式

````python
# -*- coding: gbk-2312 -*-
````

代表该文件以`gbk-2312`编码

### 标识符

和C语言规则相同

### 保留字

在`python`中的`keyword`模块，可以输出当前版本的所有保留字

```python
import keyword
keyword.kwlist

# 使用python3.6.3
# 输出结果:
['False', 'None', 'True', 'and', 'as', 'assert', 'break', 'class', 'continue', 'def', 'del', 'elif', 'else', 'except', 'finally', 'for', 'from', 'global', 'if', 'import', 'in', 'is', 'lambda', 'nonlocal', 'not', 'or', 'pass', 'raise', 'return', 'try', 'while', 'with', 'yield']

```

### 注释

```python
# 单行注释
'''
多行注释
还可以使用"""
'''
```

### 行与缩进

python利用缩进来代表代码块，不需要使用大括号`{}`

同一个代码块的缩进空格数必须相同

### 多行语句

和C语言相同，在末尾使用`\`可以连接下一行的语句，在`[],{},()`中，不需要使用该连接符号

```python
total = one+ \
		two+ \
    	three
```

### 同一行显示多条语句

在每条语句之间用`;`相隔开

```python
import sys;x = "zcc"; sys.stdout.write(x+"\n")

# 输出结果
# zcc
```



### print输出

默认换行，在其末尾加上`end=""`就不会换行

```python
print("first")
print("first_2",end="")
print("second")

# 输出结果为:
# first
# first_2second
```



### import和from...import

在 python 用 **import** 或者 **from...import** 来导入相应的模块。

将整个模块(`somemodule`)导入，格式为： **import somemodule**

从某个模块中导入某个函数,格式为： **from somemodule import somefunction**

从某个模块中导入多个函数,格式为： **from somemodule import firstfunc, secondfunc, thirdfunc**

将某个模块中的全部函数导入，格式为： **from somemodule import \***

```python
import sys #导入模块sys
from sys import argv,path #从模块sys中导入argv和path两个函数
from time import *  #导入模块sys的所有函数
```



### 命令行参数

在执行python时，可以接收命令行输入的参数



1. 可以使用`sys`模块的`argv`函数获取命令行参数

2. 使用`getopt`模块

   

## 标准数据类型

### 引言

内置的` type() `函数可以用来查询变量所指的对象类型。

### 数字

  **可以使用`del`删除对象的引用**

#### 整数

`python`允许在数字中间用`_`分割，例如：`1_000_000_000`



**Python的整数没有大小限制**

#### 浮点数

可以使用科学计数法，例如：`1230000=1.23e6`,`0.0000056=5.6e-6`

**浮点数进行运算时会出现误差**

Python的浮点数也没有大小限制，但是超出一定范围就直接表示为`inf`（无限大）

#### 复数complex

用`a+bj`或`complex(a,b)`表示复数`a+bi`

### 字符串

python中字符串是以`Unicode`编码的

由单引号或双引号括起来的任意文本，可以使用`\`转义出单引号--- **和C语言的转义字符相同**

Python还允许用`r''`表示`''`内部的字符串默认不转义

```python
print(r'\\\t\\')

#输出结果: \\\t\\
```



字符串的截取语法格式：`str[start : end : step]`

```python
str1="abcdefghi"
str2=str1[0:4:2]
str3=str1[0:]
print("str2 = "+str2)
print("str3 = "+str3)
# 输出结果为:
str2 = ac
str3 = abcdefghi
```



```python
# r,即raw,会自动将反斜杠转义
print(r"\n")
# 输出结果为
# \n

```

字符串中`*`的使用

```python
str1 = "abc"
str2 = str1*2
print(str2)

# 输出结果:
# abcabc
```



### 列表: List

`list`是有序(存储顺序和输入顺序相同)的集合，可以随时添加和删除其中的元素

```python
list1=[] #定义一个空列表
list2=[0,9,8]
list3=[0,'a',100]
```

可以通过索引查找列表中对应的元素

```python
List=[0,1,2,3,4,5,6,7,10]
print(len(List))
print(List[1])
print(List[-1])

'''
输出结果:
9
1
10
'''

```



常用函数介绍：

`len()` :获取长度---不只是列表

`append(x)` :在列表末尾插入`x`

`insert(der,x)` :在`der`处插入元素`x`

`pop()` :返回列表的末尾元素

```python
L=[]
len(L) #获取列表L的长度
L.append(1) #在列表L末尾插入1
L.insert(0,9) #将9插入到列表中的0处，如果该位置非空，那么这个数据向后移
print(L.pop()) #返回并删除末尾元素
```

### 元组

### 集合

### 字典



## 变量与常量

### 变量

在Python中，等号`=`是赋值语句，可以把任意数据类型赋值给变量，

**同一个变量可以反复赋值，而且可以是不同类型的变量**

```python
a=123
a='ABC'#该表达式合法

```

### 常量

python没有对常量的支持，但是一般常量都是用大写字母表示
