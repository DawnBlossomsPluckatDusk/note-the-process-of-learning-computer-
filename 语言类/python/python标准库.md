# python常用标准库

> 参考： [标准库简介](https://docs.python.org/zh-cn/3.8/tutorial/stdlib.html)

## `dir()`和`help()`

`dir`函数可以查看库中包含的全部函数

```python
import os
print(dir(os))
```

`help`函数则可以查看库的文档

```python
import os
help(os)
```



## `os`库

> 提供了一种使用与操作系统相关的功能的便捷式途径

如果使用无效或无法访问的文件名与路径，或者其他类型正确但操作系统不接受的参数，此模块的所有函数都抛出 `OSError` （或者它的子类）

### `os.name`

导入的依赖特定操作系统的模块的名称。以下名称目前已注册: `'posix'`, `'nt'`, `'java'`.

```python
import os
print(os.name)

# Windows下会出现'nt'
```

### `os.environ`

字典类型数据，存储系统的环境变量

```python
for key in os.environ.keys():
	print(key)
# 获取操作系统下的key字段
```

Windows常见的`key`：

```python
os.environ['HOMEPATH']:当前用户主目录。
os.environ['TEMP']:临时目录路径。
os.environ["PATHEXT"]:可执行文件。
os.environ['SYSTEMROOT']:系统主目录。
os.environ['LOGONSERVER']:机器名。
os.environ['PROMPT']:设置提示符。
```

Linux常见的`key`：

```python
os.environ['USER']:当前使用用户。
os.environ['LC_COLLATE']:路径扩展的结果排序时的字母顺序。
os.environ['SHELL']:使用shell的类型。
os.environ['LAN']:使用的语言。
os.environ['SSH_AUTH_SOCK']:ssh的执行路径。
```

### `os.getcwd()`

获取当前的工作目录

```python
import os
print(os.getcwd())
```

### `os.chdir(path)`

切换到指定`path`下

### `os.makedirs(path, mode=0o777)`

用于递归创建目录

### `os.mkdir(path[,mode])`

以数字权限模式创建目录。默认的模式为 0777 (八进制)

### `os.listdir(path)`

列出`path`目录下所有文件和文件夹

### `os.remove(path)`

用于删除指定路径的文件。如果指定的路径是一个目录，将抛出`OSError`