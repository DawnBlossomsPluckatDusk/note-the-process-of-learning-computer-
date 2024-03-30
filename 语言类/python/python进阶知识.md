# 函数

> 参考：[Python 3.8.18 文档](https://docs.python.org/zh-cn/3.8/tutorial/controlflow.html#defining-functions)

## 函数定义

```python

def func(param):
    '''
	func是函数名
	param是参数列表
	'''
    pass
```

**函数内的第一条语句是字符串时，该字符串为文档字符串，可以自动生成在线文档**



## 函数参数

几种参数形式：

1. 默认值参数

   给参数指定默认值

   ```python
   def func(name,age,sex='1',tel):
       '''
       这里的sex参数为默认值参数
       调用时可以不用给出sex参数的值
       '''
       pass
   ```

   **默认值只计算一次，当默认值为可变参数如列表时，调用函数会得到不同的效果**

2. 关键字参数

   参数为`key = value`的形式

   ```python
   def func(name,age=20,sex='1'):
       pass
   
   #调用函数
   func('zxc')		# 位置参数：对应位置的参数
   func(name='zxc')	# 关键字参数
   func('zxc',sex='0',age=22)	# 位置参数，关键字参数
   ```

   **关键字参数必须在位置参数之后**

   **不能对同一个参数多次赋值**

3. 特殊参数

   ```python
   def f(pos1, pos2, /, pos_or_kwd, *, kwd1, kwd2):
         -----------    ----------     ----------
           |             |                  |
           |        Positional or keyword   |
           |                                - Keyword only
            -- Positional only
   # /和*是可选的 
   # 在 / 之前的仅位置参数
   # 在 * 之后的仅关键字参数
   ```

   **当函数定义中未使用`/`和`*`时，参数可以按位置或关键字传递**

4. 任意参数

   ```python
   def concat(*args, sep="/"):
       '''
       一般*args出现在最后
       如果后面有其他参数，仅限关键字参数
       '''
   	return sep.join(args)
   ```

   

   ```python
   def func(name,age,sex,*score,**info):
       '''
       *score形参接收一个元组
       **info形参接收一个字典
       '''
       pass
   ```

   **`*score`必须在 `**info`之前**

5. 解包实参列表

   函数调用要求独立的位置参数，但实参在列表或元组里时，要执行相反的操作

   ```python
   >>> list(range(3, 6))            # normal call with separate arguments
   [3, 4, 5]
   >>> args = [3, 6]
   >>> list(range(*args))            # 用 * 操作符把实参从列表或元组解包
   [3, 4, 5]
   ```

   如果是字典解包的话则需要使用`**`



## lambda表达式

```python
lambda x: x + 1
# x是形参
# 函数返回x+1
# lambda也是一个函数，可以包含if语句
```

## 函数注解

```python
def func(name:str,age:int,sex:str='1') -> str:
    '''
    name形参期望获取str类型的值
    func会返回一个str类型的值
    '''
    pass
```

# 装饰器

> 参考：[函数和装饰器 ](https://pythonhowto.readthedocs.io/zh-cn/latest/decorator.html#id25)

> 闭包：如果在一个内部函数中，对定义它的外部函数的作用域中的变量进行了引用，那么这个子函数就被认为是闭包

**当内嵌函数作为返回值传递给外部变量时**，将会把定义它时涉及到的引用环境和函数体自身复制后**打包成一个整体返回**，这个整体就像一个封闭的包裹



### 装饰器函数

1. 无参装饰器

   ```python
   def log(func):
       def wrapper(*args, **kwargs):
           ret = func(*args, **kwargs)
           logging.debug('%s is called' % func.__name__)
           return ret
       return wrapper
   
   func = log(func)
   func(0)
   
   ```

   使用`python`的语法糖`@`

   ```python
   @log   # 添加装饰器 log()
   def func2(n):
       print("from func2(), n is %d!" % (n), flush=True)
   
   func2(0)
   '''
   相当于
   func2 = log(func2)
   func2(0)
   '''
   ```

2. 含参装饰器

   ```python
   def log(level='debug'):
       def decorator(func):
           def wrapper(*args, **kwargs):
               ret = func(*args, **kwargs)
               if level == 'warning':
                   logging.warning("{} is called".format(func.__name__))
               else:
                   logging.debug("{} is called".format(func.__name__))
               return ret
           return wrapper
       return decorator
   
   @log(level="warning") # 添加带参数的装饰器 log()
   def func(n):
       print("from func(), n is %d!" % (n), flush=True)
   
   func(0)
   '''
   相当于
   func = log("warning")(func)
   func(0)'''
   ```

3. 类方法装饰器

   类方法拥有一个默认的形参`self`

   ```python
   import time
   def decorator(func):
       def wrapper(self, *args, **kwargs):
           start_time = time.time()
           ret = func(self, *args, **kwargs)
           end_time = time.time()
           print("%s.%s() cost %f second!" % (self.__class__,
                 func.__name__, end_time - start_time))
           return ret
       return wrapper
   
   class TestDecorator():
       @decorator
       def mysleep(self, n):
           time.sleep(n)
   
   obj = TestDecorator()
   obj.mysleep(1)
   ```

   

### 装饰器类

> 相比装饰器函数，装饰器类具有更大灵活性，高内聚，封装性特点

**装饰器类必须定义`__call_()`方法，它将一个类实例变成一个用于装饰器的方法。**

1. 无参的装饰器类

   ```python
   class Tracer():
       def __init__(self, func):
           self.func = func
           self.calls = 0
       def __call__(self, *args, **kwargs):
           self.calls += 1
           print("call %s() %d times" % (self.func.__name__, self.calls))
           return self.func(*args, **kwargs)
   
   @Tracer
   def test_tracer(val, name="default"):
       print("func() name:%s, val: %d" % (name, val))
   
   for i in range(2):
       test_tracer(i, name=("name" + str(i)))
   ```

   **装饰器类不能用于装饰类的方法**，因为 `__call__() `的第一个参数必须传递装饰器类 Tracer 的实例

2. 含参的装饰器类

   ```python
   class Tracer():
       def __init__(self, arg0): # 可支持任意参数
           self.arg0 = arg0
           self.calls = 0
       def __call__(self, func):
           def wrapper(*args, **kwargs):
               self.calls += 1
               print("arg0:%d call %s() %d times" % (self.arg0, func.__name__, self.calls))
               return func(*args, **kwargs)
           return wrapper
   
   @Tracer(arg0=0)
   def test_tracer(val, name="default"):
       print("func() name:%s, val: %d" % (name, val))
   
   for i in range(2):
       test_tracer(i, name=("name" + str(i)))
   
   ```

   装饰器类的参数需要通过类方法 ``__init__()`` 传递，所以被装饰的函数就只能在 ``__call__()`` 方法中传入，为了把函数的参数传入，必须在 ``__call__()`` 方法中再封装一层

### 类装饰器

> 类装饰器： 对类进行装饰的函数或者类
>
> 如果要装饰一个类，那么就要把类传递给装饰器

1. 使用函数装饰类

   ```python
   class DotClass():
           pass
   
   def class_add_method(Class):
       '''
       使用函数动态的为它添加类成员 x 和 y，以及类方法 move()
       '''
       Class.x, Class.y = 0, 0
       def move(self, a, b):
           self.x += a
           self.y += b
           print("Dot moves to (%d, %d)" % (self.x, self.y))
   
       Class.move = move
       return Class
   
   DotClass = class_add_method(DotClass)
   dot = DotClass()
   dot.move(1, 2)
   ```

   同理，可以使用`@`语法糖为该类添加装饰器

   ```python
   @class_add_method
   class DotClass():
       pass
   ```

   也可以返回一个新的类，而不是在增加成员和方法

   ```python
   def class_add_method_new(Class):        # @语句处调用
       class Wrapper():
           def __init__(self, *args):      # 创建实例时调用
               self.wrapped = Class(*args) # 调用 DotClass.__init__
   
           def move(self, a, b):
               self.wrapped.x += a
               self.wrapped.y += b
               print("Dot moves to (%d, %d)" % (self.wrapped.x, self.wrapped.y))
   
           def __getattr__(self, name):    # 对象获取属性时调用
               return getattr(self.wrapped, name)
   
       return Wrapper
   
   @class_add_method_new
   class DotClass():           # DotClass = class_add_method_new(DotClass)
       def __init__(self):     # 在 Wrapper.__init__ 中调用
           self.x, self.y = 0, 0
   
   dot = DotClass()            # dot = Wrapper()
   dot.move(1, 2)
   print(dot.x)                # 调用 Wrapper.__getattr__
   ```

   

2. 使用带参函数装饰类

   和之前的类似，需要在原有的基础上再封装一层

### 使用类装饰类

```python
class Tracer():
    def __init__(self, Class):  # @语句处调用
        self.Class = Class

    def __call__(self, *args, **kwargs): # 创建实例时调用
        self.wrapped = self.Class(*args, **kwargs)
        return self

    def __getattr__(self, name): # 获取属性时调用
        return getattr(self.wrapped, name)

@Tracer()
class C():
    pass
```



### 装饰器嵌套

多个装饰器按照靠近被修饰函数或者类的距离，**由近及远依次执行**的。



> `functools` 模块中的 wraps 可以帮助保留这些信息。`functools.wraps` 本身也是一个装饰器，它把被修饰的函数元信息复制到装饰器函数中，这就保留了原函数的信息。


