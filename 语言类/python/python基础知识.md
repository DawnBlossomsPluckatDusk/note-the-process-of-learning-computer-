

[TOC]



# 基础语法



> 参考书籍：
>
> python编程入门指南



## 编写规则

`python`采用`PEP8`作为编码规范

> Python Enhancement Proposal `python增强建议书`

1. 每个`import`语句只导入一个模块

2. 不在行尾添加分号

3. 建议每行不超过80个字符，如果超过，建议使用`()`将多行内容隐式地连接起来 **不推荐使用反斜杠进行连接**

   以下两种情况除外：

   + 导入模块的语句
   + 注释里的URL

4. 在顶级定义之间空两行，而在方法定义之间空一行。用于分隔某些功能的位置也可以空一行

5. 运算符两侧，函数参数之间，逗号两侧建议使用空格进行分隔

6. 避免在循环中使用`+`和`+=`运算符累加字符串	-> 字符串属于不可变量，会创建不必要的临时对象

7. 适当使用异常处理结构提高程序的容错性，但是不能依赖异常处理结构

## 命名规范

1. 模块名尽量短小，并且全部使用小写字母，可以使用下划线分隔
2. 包名尽量短小，冰鞋全部使用小写字母，不推荐使用下划线
3. 类名采用单词首字母大写形式，即Pascal风格
4. 模块内部的类采用下划线+Pascal风格的类名
5. 函数，类的属性和方法的命名规则同模块类似
6. **常量命名时全部采用大写字母，可以使用下划线**
7. **使用单下划线开头的模块变量或函数是受保护的**，使用`import * from`语句时，这些变量和函数不会被导入
8. **使用双下划线开头的实例变量或方法是私有的**

## python保留字

| import   | form     | as      | and    | or    | not     |
| -------- | -------- | ------- | ------ | ----- | ------- |
| if       | elif     | else    | for    | while | in      |
| is       | continue | break   | pass   | def   | return  |
| try      | except   | finally | del    | class | None    |
| True     | False    | with    | lambda | yield | globbal |
| nonlocal | assert   | raise   |        |       |         |

```python
import keyword
keyword.kwlist
```



## python标识符

`pyhon`中以下划线开头的标识符有特殊意义

+ 以单下划线开头的标识符表示不能直接访问的类属性，也不能通过`import`语句导入
+ 以双下划线开头的标识符表示类的私有成员
+ 以双下划线开头和结尾的是python里专用的标识

变量命名规则：

+ 变量名是有效的标识符
+ 不能使用保留字
+ 慎用小写字母`i`和大写字母`O`
+ 选择有意义的单词作为变量名

在`python`中，不需要先声明，**直接赋值即可创建各种类型的变量**

> 使用`type()`函数可以判断变量的类型
>
> 使用`id()`可以获取变量的内存地址

## 基本数据类型

### 数字类型

1. 整数(**会自动使用高精度计算**)
   + 十进制
   + 二进制
   + 八进制  -> 以`0o`开头
   + 十六进制 -> 以`0x`开头
2. 浮点数(可以使用科学计数法)   -> 可能出现小数位数不确定的情况
3. 复数 -> 使用`j`或`J`表示虚部，如：`1+9.1j`



### 字符串类型

使用单引号，双引号和三引号

**字符串属于不可变序列**

**在字符串前加上`r`表示忽视转义字符，将字符串原样输出**



### 布尔类型

只有下列几种情况得到的值为假：

+ `False`或`None`
+ 数值中的零
+ 空序列
+ **自定义对象的实例，对象的`__bool__`方法返回 False 或者`__len__`方法返回 0**  



## 数据类型转换

`int()`	转换为整型

`float()`	转换为浮点数

`complex()` 	转换为复数

`str()`	转换为字符串

`repr()`	转换为表达式字符串

`eval()`	计算字符串中的有效python表达式

`chr()`	将整数转换为字符



## 运算符

### 算术运算符

`+ - * / % // **`

**使用除法运算或求余运算，除数不能为0**



### 赋值运算符

`= += -= *= /= %= **= //= `



### 比较运算符

`> < == >= <= !=`

**当需要判断变量是否介于两个值之间时，可以使用`值 < 变量 < 值`，如：`1 < x < 3`**



### 逻辑运算符

`and or not`



### 位运算符

`& | ^ ~ >> <<`

**遇到表达式乘以或除以2的n次幂的情况时，一般采用移位运算**



## 选择结构

```python
if xxxx:
    pass
elif xxx:
    pass
else:
    pass
```

```python
match expression:
    case pattern1:
        # 处理pattern1的逻辑
    case pattern2 if condition:
        # 处理pattern2并且满足condition的逻辑
    case _:
        # 处理其他情况的逻辑
```



## 循环结构

```python
while xxx:
    pass
```

```python
for xxx in xxx:
    pass
```



# 数据结构



## 序列

序列结构主要有：

+ 列表
+ 元组
+ 集合
+ 字典

**进行序列相加时，只能是相同类型的序列，即同为列表、元组、集合等**



| 函数 | 作用 |
| - | - |
|`list()`|将序列转化为列表|
|`str()`|将序列转化为字符串|
|`sorted()`|对元素进行排序|
|`reversed()`|反向序列中的元素|



## 列表	-> 	可变序列

**使用`del`删除列表时，需要保证列表名存在**



### 遍历列表

使用`for` 循环和`enumerate()`函数可以实现同时输出索引值和元素内容

```python
for index,value in enumerate(listname):
```

> enumerate() 函数用于将一个可遍历的数据对象(如列表、元组或字符串)组合为一个索引序列，同时列出数据和数据下标






### 添加元素

`append()`：在列表**末尾**添加元素

`insert()`：在列表中添加元素

`extend()`：将某个列表添加到列表末尾



### 删除元素

1. 按照索引删除
2. 按照元素值删除  -> `remove()`



### 列表排序

1. sort函数

   ```python
   listname.sort()
   ```

   

2. sorted函数

   ```python
   sorted(listname)
   ```



> 该方法对中文支持不好，如果需要实现中文排序，需要重新实现





### 常用函数

| 函数名  | 作用                             |
| ------- | -------------------------------- |
| count() | 统计某个元素出现的次数           |
| index() | 获取某个元素**第一次出现**的下标 |
| sum()   | 统计**数值列表**中各元素的和     |



### 列表推导式

1. 生成指定范围的数值列表

   ```python
   # ls = [expression for var in range()]
   ls = [2 * i for i in range(10)]
   ```

2. 根据列表生成指定需求的列表

   ```python
   # ls = [expression for var in list]
   ls1 = [1,2,3,4,5,6,7]
   ls2 = [2 * i for i in ls1]
   ```

   

3. 从列表中选择符合条件的元素组成新的列表

   ```python
   # ls = [expression for var in list if condition]
   ls1 = [1,2,3,4,5,6,7,8,9]
   ls2 = [2 * i for i in ls if i > 3]
   ```

**使用列表推导式生成二维数组**

```python
ls = [[None for i in range(5)] for j in range(10)]
```



## 元组	->	不可变序列

```python
#元组的创建
tuple1 = (1,2,3)
tuple2 = (1,)	#不能省略逗号
tuple3 = 1,2
# 字符串
string = ("zcc")
```



使用`tuple()`函数可以将可迭代类型数据(列表，`range`)转换为元组

**在进行元组的连接时，连接的内容必须都是元组**



### 元组推导式

```python
# 和列表推导式相似，只需要更改[]为()
number = (expression for var in range())
tuple1 = tuple(number)
```



**使用元组推导式生成的结果并不是一个元组或者列表，而是一个生成器对象** 



### 元组与列表的区别

+ 列表属于**可变序列**，它的元素可以随时修改或者删除，而元组属于**不可变序列**，其中的元素不可以修改，除非整体替换。

+ 列表可以使用 append()、 extend()、 insert()、 remove()和 pop()等方法实现添加和修改列表元素，而元组则没有这几个方法，因为不能向元组中添加和修改元素，同样，也不能删除元素

+ 列表可以使用切片访问和修改列表中的元素。元组也支持切片，但是它只支持通过切片访问元组中的元素，不支持修改。

+ 元组比列表的访问和处理速度快。所以如果只需要对其中的元素进行访问，而不进行任何修改，建议使用元组而不使用列表。

+ **列表不能作为字典的键，而元组则可以。**



## 字典	-> 	可变序列



字典的主要特点：

+ 通过键读取元素值
+ 字典是任意对象的无序集合
+ 字典是可变的，且任意嵌套
+ 字典中的键是**唯一**的
+ 字典的键是**不可变**的



### 字典的创建

1. 通过映射函数创建字典

   ```python
   dic = dict(zip(lis1,lis2))
   # zip函数：用于将多个列表或元组对应位置的元素组合为元组，并返回zip对象，如果长度不同，以最短的相同
   ```

2. 通过给定键值对创建字典

3. `fromkeys()`函数创建值为空的字典

   ```python
   dic = dict.fromkeys(lis)
   ```



**python推荐使用`get`方法获取指定的值**

```python
dic.get(key,[default])
# default为可选项，用于指定当键不存在时，返回一个默认值
```



## 字符串常用操作



### 字符串拼接

使用`+`拼接两个字符串

**字符串不允许直接与其他类型的数据进行拼接**



### 字符串长度

**汉字在`GBK/GB2312`编码中占`2`字节，在`UTF-8/Unicode`中一般占`3`字节(或4字节)**

使用`encode`函数可以指定编码方式

```python
str = "人生苦短，我用 Python!"
lens = len(str.encode()) 	#默认采用utf-8
```



### 分隔字符串

使用`split()`函数进行字符串分隔

```python
str1.split(sep[, maxsplit])
# sep：指定分隔符，默认为None，即空字符
# maxsplit：用于指定分隔次数
```

**如果不指定`sep`参数，也不能指定`maxsplit`**

**如果不指定参数，默认采用空格符进行分隔，这时无论有几个空格，空格符都将作为一个分隔符进行分隔**  

**如果指定一个分隔符，那么当这个分隔符出现多个时，就会每个分隔一次，没有得到内容的，将产生一个空元素**  



### 检索字符串

1. `count`函数：用于检索指定字符串在另一个字符串中出现的次数，不存在则返回`0`

   ```python
   str1.count(sub[, star[, end]])
   # sub表示要检索的子串
   ```

2. `find`函数：用于检索是否包含指定的子字符串，不存在则返回`-1`,存在则返回首次出现的索引

   ```python
   str1.find(sub[, star[, end]])
   # sub表示要检索的子串
   ```

   **如果只是想要判断指定的字符串是否存在，可以使用 `in`关键字实现**

3. `index`函数：和`find`类似，但是如果字符串不存在**会抛出异常**

   ```python
   str1.index(sub[, star[, end]])
   # sub表示要检索的子串
   ```

4. `startswith`函数：用于检索字符串是否以指定子字符串开头，**返回布尔值**

   ```python
   str1.startswith(sub[, star[, end]])
   # sub表示要检索的子串
   ```

5. `endswith`函数：用于检索字符串是否以指定子字符串结尾，**返回布尔值**

   ```python
   str1.endswith(sub[, star[, end]])
   # sub表示要检索的子串
   ```



### 去除多余的空格和特殊字符

1. `strip()`：用于去掉字符串左右两侧的空格和特殊字符

   ```python
   str1.strip([chars])
   # chars：指定要去除的字符
   ```

2. `lstrip()`：用于去掉左侧的空格和特殊字符

3. `rstrip()`：用于去掉右侧的空格和特殊字符



### 格式化字符

1. 使用`%`操作符

   ```python
   '%[-][+][0][m][.n]格式化字符'%exp
   # -: 指定左对齐，正数无符号，负号前加-
   # +: 指定右对齐，正数加+，负号加-
   # 0: 表示右对齐，正数无符号，负数加-，用0填充空白
   # m: 表示占用宽度
   # .n: 表示小数点后保留的位数
   # 格式化字符: 用于指定类型
   # exp: 要转换的项，可以使用元组，不能使用列表
   ```

   | 格式化字符 | 说明                   |
   | ---------- | ---------------------- |
   | %s         | 字符串                 |
   | %c         | 单个字符               |
   | %d或%i     | 十进制整数             |
   | %x         | 十六进制               |
   | %f或%F     | 浮点数                 |
   | %r         | 字符串(采用repr()显示) |
   | %o         | 八进制                 |
   | %e或%E     | 指数                   |

   

   ```python
   template = '编号： %09d\t 公司名称： %s \t 官网： http://www.%s.com' # 定义模板
   context1 = (7,'百度','baidu') # 定义要转换的内容 1
   context2 = (8,'明日学院','mingrisoft') # 定义要转换的内容 2
   print(template%context1) # 格式化输出
   print(template%context2) # 格式化输出
   ```

   

2. 使用字符串对象的`format()`

   ```python
   str1.format(args)
   # format 用于指定字符串的显示模板
   # args 用于指定要转换的项
   ```

   **需要使用`{}`和`:`指定占位符**

   ```c
   {[index][:[[fill]align][sign][#][width][.precision][type]]}
   // index: 用于指定要设置格式的对象在参数列表中的索引位置
   // fill: 用于指定空白处填充的字符
   // align: 用于指定对齐方式, < 表示左对齐; > 表示右对齐; = 表示右对齐，符号在最左侧; ^ 表示居中
   // sign: 用于指定有无符号数, + 表示正数加+，负数加-; - 表示正数不变，负数加-; 空格表示正数加空格,负数加-
   // #: 对于二进制、八进制和十六进制，会显示/0b/0o/0x前缀
   // width: 用于指定所占宽度
   // .precision: 用于指定保留的小数位数
   // type: 用于指定类型，如: s 表示字符串; d 表示十进制数
   ```

   ```python
   template = '编号： {:0>9s}\t 公司名称： {:s} \t 官网： http://www.{:s}.com' # 定义模板
   context1 = template.format('7','百度','baidu') # 转换内容 1
   context2 = template.format('8','明日学院','mingrisoft') # 转换内容 2
   print(context1) # 输出格式化后的字符串
   print(context2) # 输出格式化后的字符串
   ```

   



## 正则表达式

### 行定位符

`^` : 表示行的开始

`$`: 表示行的结尾

### 元字符

`.`: 匹配除换行符意外的任意字符

`\w`: 匹配字母、数字、下划线或汉字

`\s`: 匹配任意的空格符

`\d`: 匹配数字

`\b`: 匹配单词的开始或结束

### 限定符

| 限定符 | 说明                                   |
| ------ | -------------------------------------- |
| ?      | 匹配前的字符**0次或1次**               |
| +      | 匹配前面的字符**1次或多次**            |
| *      | 匹配前面的字符**0次或多次**            |
| {n}    | 匹配前面的字符n次                      |
| {n,}   | 匹配前面的字符**最少**n次              |
| {n,m}  | 匹配前面的字符**最少**n次，**最多**m次 |

### 字符类

使用`[]`匹配字符

```reStructuredText
[?.,]	# 匹配标点?.,
[0-9]	# 匹配一位数字
[^a-zA-Z]	#排除字母，即匹配一个不是字母的字符
```

### 选择字符

`|`: 表示为或

```html
/(^\d{15}$)|(^\d{18}$)|(^\d{17}(\d|X|x)$)/
# 匹配 15 位数字、 18 位数字，或者 17 位数字和最后一位。最后一位可以是数字或者 X（ x）
```

> 括号在正则表达式中也算是一个元字符  

### 分组

小括号的第一个作用就是可以改变限定符的作用范围  

小括号的第二个作用是分组，也就是子表达式。如`(\.[0-9]{1,3}){3}`就是对分组`(\.[0-9]{1,3})`进行重复操作  



## `re`模块

1. 匹配字符串

   + 使用`match()`方法

     match()方法用于**从字符串的开始处进行匹配**，如果在起始位置匹配成功，则返回 Match对象，否则返回 None

     ```python
     re.match(pattern, string, [falgs])
     # pattern: 表示模式字符串，表示正则表达式
     # string: 标配是要匹配的字符串
     # flags: 表示标志位，用于控制匹配方式
     ```

     | 标志          | 说明                                                 |
     | ------------- | ---------------------------------------------------- |
     | A(ASCII)      | 对于\w,\W,\b,\B,\d,\D,\s和\S只进行ASCII匹配          |
     | I(IGNORECASE) | 执行不区分字母大小写的匹配                           |
     | M(MULTILINE)  | 将`^`和`$`用于包括整个字符出啊你的开始和结尾的每一行 |
     | S(DOTAIL)     | 使用`.`字符匹配所有字符，包括换行符                  |
     | X(VERBOSE)    | 忽略模式字符串中为转移的空格和注释                   |

     ```python
     import re
     pattern = r'mr_\w+' # 模式字符串
     string = 'MR_SHOP mr_shop' # 要匹配的字符串
     match = re.match(pattern,string,re.I) # 匹配字符串，不区分大小写
     print(match) # 输出匹配结果
     string = '项目名称 MR_SHOP mr_shop'
     match = re.match(pattern,string,re.I) # 匹配字符串，不区分大小写
     print(match) # 输出匹配结果
     ```

   + 使用`search()`方法进行匹配

     search()方法用于在**整个字符串中搜索第一个匹配的值**，如果在起始位置匹配成功，则返回 Match 对象，否则返回 None  

     `re.match(pattern, string, [falgs])`

   + 使用`findall()`方法进行匹配

     findall()方法用于在**整个字符串中搜索所有符合正则表达式的字符串**，并以列表的形式返回。如果匹配成功，则返回包含匹配结构的列表，否则返回空列表  

     `re.match(pattern, string, [falgs])`

2. 替换字符串

   ```python
   re.sub(pattern, repl, string[, count, flags])
   # pattern: 表示模式字符串，表示正则表达式
   # repl: 表示替换的字符串
   # string: 标配是要匹配的字符串
   # count: 表示模式匹配后替换的最大次数
   # flags: 表示标志位，用于控制匹配方式
   ```

   ```python
   import re
   pattern = r'1[34578]\d{9}' # 定义要替换的模式字符串
   string = '中奖号码为： 84978981 联系电话为： 13611111111'
   result = re.sub(pattern,'1XXXXXXXXXX',string) # 替换字符串
   print(result)
   ```

   

3. 分隔字符串

   split()方法用于实现根据正则表达式分隔字符串，**并以列表的形式返回**  

   ```python
   re.split(pattern, string, [maxsplit], [flags])  
   # pattern: 表示模式字符串，表示正则表达式
   # string: 标配是要匹配的字符串
   # maxsplit: 表示最大的拆分次数
   # flags: 表示标志位，用于控制匹配方式
   ```

   ```python
   import re
   pattern = r'[?|&]' # 定义分隔符
   url = 'http://www.mingrisoft.com/login.jsp?username="mr"&pwd="mrsoft"'
   result = re.split(pattern,url) # 分隔字符串
   print(result)
   ```

   

# 异常处理



### 常见异常

| 异常              | 说明                       |
| ----------------- | -------------------------- |
| NameError         | 尝试访问一个没有声明的变量 |
| IndexError        | 索引超出序列范围           |
| IndentationError  | 缩进错误                   |
| ValueError        | 传入的值错误               |
| KeyError          | 请求一个不存在的字典关键字 |
| IOError           | 输入输出错误               |
| ImportError       | `import`语句无法找到模块   |
| AttributeError    | 尝试访问未知的对象属性     |
| TypeError         | 类型不适合                 |
| MemoryError       | 内存不足                   |
| ZeroDivisionError | 除数为`0`                  |



### 异常处理程序



#### `try except`语句

**把可能产生异常的代码放在 try 语句块中，把处理结果放在 except 语句块中**  

```python
try:
    pass
except [ExceptionName [as alias]]:
    pass
# ExceptionName [as alias]: 用于指定要捕获的异常, ExceptionName表示异常名称,如果在其右侧加上 as alias 则表示为当前的异常指定一个别名，通过该别名，可以记录异常的具体内容
```

在使用 try…except 语句捕获异常时，如果在 except 后面不指定异常名称，则表示**捕获全部异常**  

使用 try…except 语句捕获异常后，当程序出错时，输出错误信息后，**程序会继续执行**  



#### `try except else`语句

`else`子句用于指定当`try`中没有发现异常时要执行的语句块

```python
try:
    pass
except:
    pass
else:
    pass
```



#### `try except finally`语句

无论程序中有无异常产生， **`finally` 代码块中的代码都会被执行**  

```python
try:
    pass
except:
    pass
else:
    pass
finally:
    pass
```



### 抛出异常

使用`raise`语句抛出异常

```python
raise [ExceptionName [(reason)]]
# ExceptionName [(reason)]: 用于指定抛出的异常名称，以及异常信息的相关描述
```



### 调试程序

使用`assert`语句进行调试，一般用于对某个时刻必须满足的条件进行验证

```python
assert expression [,reason]
# expression: 条件表达式，如果表达式为假，抛出AssertionError；否则什么都不做
# reason: 用于对判断条件进行描述
```

>  assert 语句只在调试阶段有效。我们可以通过在执行 python 命令时加入-O（大写字母）参数来关闭 assert 语句  



# 函数



## 参数传递

在`python`中，**当实际参数为不可变对象时，进行的是值传递；当实际参数为可变对象时，进行的是引用传递**  

> 实际参数：将函数的调用者提供给函数的参数
>
> 形式参数：定义函数时，函数名后面括号中的参数



## 关键字参数

关键字参数是指**使用形式参数的名字**来确定输入的参数值，只需要参数名称正确即可



## 参数默认值

在定义函数时，**指定默认的形式参数**必须在**所有参数的最后**，否则将产生语法错误 

> 使用`函数名.__default__`可以查看函数的默认值参数的当前值，其结果是一个元组

使用可变对象作为函数参数的默认值时，**多次调用可能会导致意料之外的情况** ，**最好使用 None 作为可变对象的默认值，这时还需要加上必要的检查代码**  



## 可变参数

1. `*parameter`

   表示接受任意多个实际参数并将其放到一个**元组**中

   ```python
   def pri(*name):
       for i in name:
           print(i)
           
   pri('z','c','d') 
   ```

   如果想要使用一个已经存在的列表作为函数的可变参数，可以**在列表的名称前面加`*`**

   ```python
   ls = ['z','c','f']
   pri(*ls)
   ```

   

2. `**parameter`

   表示接受任意多个类似关键字参数一样显式赋值的实际参数，并将**其放到一个字典中**

   如果想要使用一个已经存在的字典作为函数的可变参数，可以**在字典的名称前加`**`**



## 返回值

当函数中没有 `return`语句时，或者省略了`return` 语句的参数时，**将返回`None`，即返回空值**。  



## 变量的作用域

**如果一个变量在函数体内定义，并且使用`global`关键字修饰后，该变量为全局变量**



## 匿名函数(lambda)

使用 lambda 表达式时，参数可以有多个，用逗号“,”进行分隔，但是**表达式只能有一个**，即只能返回一个值，而且也**不能出现其他非表达式语句**（如 for 或 while）  



# 面向对象

面向对象中的对象（ Object）通常是指客观世界中存在的对象，这个对象具有唯一性，对象之间各不相同，各有各的特点，每一个对象都有自己的运动规律和内部状态；对象与对象之间又是可以相互联系、相互作用的。  



## 基本特征

1. 封装
2. 继承
3. 多态

> 在`python`中创建实例不使用`new`关键字



## `__init()__`方法

> 当使用类创建实例时，都会执行一次

**`__init()__`必须包含一个参数`self`，并且是第一个参数**

> `self`参数是指向实例本身的引用，用于访问类中的属性和方法

```python
class stu:
    def __init__(self):		# 构造方法
        self.x = 1
st = st()		# 创建实例
```



## 实例方法

> 实例方法是指在类中定义的函数

**实例方法的第一个参数必须是`self`，并且必须包含一个参数`self`**



## 数据成员

> 数据成员是指在类中定义的变量，即属性

1. 类属性

   定义在类中，**并且在函数体外的属性**。**类属性可以在类的所有实例之间共享值**

   > 类属性可以通过类名或实例名访问

   ```python
   class stu:
       name = 'zcc'
       sex = 1
       def __init__(self):
           print(stu.name)
   ```

   

2. 实例属性

   定义在类的方法中的属性，**只作用于当前实例中**

   ```python
   class stu:
       def __init__(self):
           self.name = "zcc"
           self.sex = 1
   ```



## 访问限制

在属性或方法名前面添加**单下划线、双下划线或者首尾加双下划线**，从而限制访问权限

`__foo__`：一般是系统定义名字

`_foo`：表示`protected`类型的成员，**只允许类本身和子类进行访问**，不能使用`from module import *`语句导入

`__foo`：表示`private`类型的成员，**只允许定义该方法的类本身进行访问**，并且也不能通过类的实例进行访问，可以通过`实例名._类名__xxx`访问



## 属性

> 通过`@property`(装饰器)将一个方法转换为属性，从而实现用于计算的属性

```python
class stu:
    def __init__(self):
        self.x = 1
        self.y = 2
    @property
    def site(self):
        return (self.x,self.y)
st = stu()      
st.site
```

**通过`@property`转换后的属性不能重新赋值**



### 使用`@property`实现只读属性

```python
class stu:
    def __init__(slef,name):
        self.__name = name
    @property
    def show(self):
        return self.__name
st = stu("zcc")
print(st.show)
```



## 继承

在类定义语句中的类名右侧使用一对小括号将要继承的基类名称括起来，从而实现类的继承

```python
class stu(human):
    pass
```

**在派生类中定义`init()`方法时，不会自动调用基类的`init()`方法。**  

**要让派生类调用基类的`init()`方法进行必要的初始化，需要在派生类中使用`super()`函数调用基类的`init()`方法。**  

```python
super.__init__()	#调用父类的init方法
```



# 模块

> 一个`py`文件就是一个模块

**在创建模块时，设置的模块名不能是`python`自带的标准模块名**，如果相同，那么将不会导入`python`的标准模块

> 使用`import`语句导入模块时，每执行一条`import`语句都hi创建一个新的命名空间，并且在该命名空间执行与`py`文件相关的所有语句

**使用`from import`不会创建新的命名空间**

> 命名空间可以理解为记录对象名字和对象之间对应关系的空间。`python`的命名空间是通过字典实现的

**可以使用`dir`函数查看导入的函数**

使用`from import`导入模块中的定义，**需要保证导入的内容在当前命名空间是唯一的**，不然后导入的会覆盖之前导入的



### 模块搜索目录

当使用`import`语句导入模块时，在默认情况下，会按照以下顺序进行查找

1. 在当前目录查找
2. 到`pythonpath`(环境变量)下的每个目录查找
3. 到`python`的默认安装目录查找

**使用`sys.path`变量可以查看这些具体位置**



### 添加指定目录到`sys.path`

1. 临时添加

   使用`append`函数

   ```python
   import sys
   sys.path.append("xxx")
   ```

   **通过该方法添加的目录只在执行当前文件的窗口中有效**

2. 增加`.pth`文件

   在`python`安装目录下的`Lib\site-packages`子目录中，创建一个扩展名为`.pth`的文件，文件名任意。在该文件中添加要导入模块所在的目录

   **需要重新打开执行导入模块的`python`文件**

   **通过该方法添加的目录只在当前版本的`python`有效**

3. 在`pythonpath`环境变量中添加

   **通过该方法添加的目录可以在不同`python`版本中共享**



### 主程序

在每个模块的定义中都包括一个记录模块名称的变量`__name__`，程序可以检查该变量，以确定它们在哪个模块中执行。  如果一个模块不是被导入其他程序中执行的，那么它可能在解释器的顶级模块中执行。顶级模块的`__name__`变量的值为`__main__`



## 包

> 包是一个分层次的目录结构，他将一组功能相近的模块组织在一个目录下，避免了模块名重名的冲突

**在包下必须存在一个名称为`__init__.py`的文件**



### 包的创建

> 创建包也就是创建一个文件夹，并且在该文件夹中创建一个名称为`__init__.py`的文件，`__init__.py`会在导入包时自动执行



### 包的使用

> 使用`import`语句从包中加载模块

1. 通过`import+完整包名+模块名`的方式加载指定模块

2. 通过`from+完整包名+import+模块名`的方式加载指定模块

3. 通过`from+完整包名+import+模块名+定义名`的方式加载指定模块

   **而可以使用`*`代替定义名，表示加载该模块下的全部定义**

# 文件及目录



## 文件常用操作


### 打开和创建文件

使用`open`函数

```python
file = open(filename[,mode[,buffering]])
# buffering: 用于指定读写文件的缓冲模式，值为0表示不缓存，值为1表示缓存，值大于1表示缓冲区的大小，默认为缓存模式
```

对于非文本文件，需要使用**二进制形式**打开

**使用`open`函数默认使用GBK编码**



**打开文件之后需要及时关闭，避免对文件造成不必要的破坏**

> `close()`方法先刷新缓冲区中还没有写入的信息，再关闭文件



 ### 使用`with`语句

> 使用`with`语句可以实现在处理文件时，无论是否抛出异常，都唔那个保证`with`语句执行完毕后关闭已经打开的文件

```python
with expression as target:
    body
```

```python
with open("user.txt","w") as file:
    pass
```



### 读写文件

写文件使用`write()`函数

读文件：

1. `read([size])`: 读取size个字符，无参数则表示读取全部
2. `readline()`: 读取一行
3. `readlines()`：读取全部行，**返回字符串列表**

使用`seek()`方法可以将文件的指针移动到新的位置

```python
file.seek(offest[,whence])
# offest: 用于指定移动的字符个数
# whence: 用于指定从什么位置开始计算，0表示从文件头开始, 1表示从当前位置开始, 2表示从文件尾开始计算, 默认为0
```

**如果在打开文件时，没有使用`b`模式，那么只允许从文件头开始**



## 目录操作

> `os`模块时`python`内置的与操作系统功能和文件系统相关的模块

**如果`os.name`的输出结果时`nt`，则表示的是Windows；如果是`posix`，则表示是Linux、Unix或Mac**

`linesep`变量：用于获取当前操作系统上的换行符

`sep`变量：用于获取当前操作系统所使用的路径分隔符

> 指定文件路径时需要对路径分隔符进行转义，如`demo\\message.txt`;也可以在字符串前加上`r`



### 拼接路径

使用`join`函数

```python
os.path.join(path1[,path2[,...]])
```

**使用`join`拼接路径时，不会检测路径是否存在**

**如果要拼接的路径中存在多个绝对路径，那么以从左到右最后一次出现的为准，并且该路径之前的参数都将被忽略**  

**当把两个路径拼接为一个路径时，不要直接使用字符串拼接，而是使用os.path.join()函数，这样可以正确处理不同操作系统的路径分隔符**  



**`os.path.exists()`函数除了可以判断目录是否存在，还可以判断文件是否存在**  

**使用`rmdir()`函数只能删除空的目录，如果想要删除非空目录，则需要使用 Python 内置的标准模块 `shutil` 的 `rmtree()`函数实现**  

**在使用`rename()`函数重命名目录时，只能修改最后一级的目录名称  **
