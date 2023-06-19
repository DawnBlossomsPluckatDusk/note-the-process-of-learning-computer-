[TOC]

# 时间线

1998	c++98	

2003	c++03	

2011	c++11	

2014	c++14

2017	c++17

2020	c++20



# 类型转换

## 静态转换

将一种数据类型的值转换为另一种数据类型的值

通常用于比较类型相似的对象之间的转换，在转换时**不会进行类型检查**

```c++
int i = 10;
float f = static_cast<float>(i);	//将int类型转换为float
```

## 动态转换

通常用于将一个基类指针或引用转换为派生类指针或引用。转换时，**会进行类型检查**。

```c++
class base{};
class driver : public base {};
base* ptr_base = new driver;
driver* ptr_driver = dynamic_cast<driver*>(ptr_base)
```

## 常量转换

用于将`const`类型的对象转换为非`const`类型的对象。只能用于转换掉`const`属性，不能改变类型

```c++
const int i = 1;
int& r = const_cast<int&>(i);
```

## 重新解释转换

将一个数据类型的值重新解释为另一个数据类型的值

通常用于在不同的数据类型之间转换。不会进行类型检查

```c++
int i = 10;
float f = reinterpret_cast<float&>(i);
```

# 作用域

## 局部作用域

在函数或一个代码块内部声明的变量，称为局部变量。

它们只能被函数内部或者代码块内部的语句使用。

## 全局作用域

在所有函数外部定义的变量（通常是在程序的头部），称为全局变量。

全局变量的值在程序的整个生命周期内都是有效的。全局变量可以被任何函数访问

## 块作用域

块作用域指的是在代码块内部声明的变量

## 类作用域

类作用域指的是在类内部声明的变量



```c++
class Myclass{
	//类作用域变量
    public:
    	static int class_var;
}

int Myclass::class_var = 30;

//全局变量
int a =	1; 
char b;


int main(){
    //局部变量
    int c = 2;
    //块作用域
    {
        int a = 4;	//屏蔽外部的变量a
    }
    return 0;
}
```



# 类型限定符

用于在定义变量或函数时改变它们的默认行为

| 限定符   | 含有                                                         |
| -------- | ------------------------------------------------------------ |
| const    | 定义常量，表示该变量的值不能被修改                           |
| volatile | 该变量的值可能会被程序以外的因素改变，如硬件或其他线程       |
| restrict | 该指针是唯一访问它所指向的对象的方式。c99增加                |
| mutable  | 表示类中的成员变量可以在const成员函数中被修改                |
| static   | 用于定义静态变量，表示该变量的作用域仅限于当前文件或当前函数内，不会被其他文件或函数访问。 |
| register | 用于定义寄存器变量，表示该变量被频繁使用，可以存储在CPU的寄存器中，以提高程序的运行效率。 |



```c++
// const
const int NUM = 10;

//volatile
volatile int num = 20;

//mutable
class Example{
    public:
    	int get_value() const {
            return value_;	//const表示不会秀爱对象中的数据成员
        }
    	void set_value(int value) const{
            value_ = value;
        }
    private:
    	mutable int value_;
}
```

# 存储类

存储类定义 C++ 程序中变量/函数的范围（可见性）和生命周期

## auto

用于下列两种情况：

+ 声明变量时根据初始化表达式自动推断该变量的类型

+ 声明函数时函数返回值的占位符

```c++
auto f = 3.14;
auto z = new auto(9);
```

**C++98标准中auto关键字用于自动变量的声明，但由于使用极少且多余，在 C++17 中已删除这一用法。**

## register

用于定义存储在寄存器中而不是 RAM 中的局部变量

寄存器只用于需要快速访问的变量

## static

指示编译器在程序的生命周期内保持局部变量的存在，而不需要在每次它进入和离开作用域时进行创建和销毁

当 static 修饰全局变量时，会使变量的作用域限制在声明它的文件内

在 C++ 中，当 static 用在类数据成员上时，会导致仅有一个该成员的副本被类的所有对象共享



## extern

用于提供一个全局变量的引用，全局变量对所有的程序文件都是可见的

extern 修饰符通常用于当有两个或多个文件共享相同的全局变量或函数的时候



## mutable

mutable 说明符仅适用于类的对象.它允许对象的成员替代常



## thread_local

使用 thread_local 说明符声明的变量仅可在它在其上创建的线程上访问。 变量在创建线程时创建，并在销毁线程时销毁。 每个线程都有其自己的变量副本。

thread_local 不能用于函数声明或定义



# 函数

## 函数参数

调用函数时，有三种函数传递参数的方式：

+ 传值调用

+ 指针调用

  ```c++
  void swap(int *a, int *b){
      *a = *a ^ *b;
      *b = *a ^ *b;
      *a = *a ^ *b;
      return ;
  }
  int main(){
  	int a=10,b=100;
      swap(&a,&b);
      return 0;
  }
  ```

  

+ 引用调用

  ```c++
  void swap(int &a,int &b){
      a = a ^ b;
      b = a ^ b;
      a = a ^ b;
      return ;
  }
  int main(){
      int a=10,b=100;
      swap(a,b);
  }
  ```

## 参数默认值

参数列表中后边的每一个参数指定默认值。

当调用函数时，如果实际参数的值留空，则使用这个默认值。



## lambda函数

C++11 提供了对匿名函数的支持

Lambda 表达式可以像对象一样使用，比如可以将它们赋给变量和作为参数传递，还可以像函数一样对其求值

Lambda 表达式具体形式如下:

```
[capture](parameters)->return-type{body}
```

```c++
[](int x, int y){ return x < y ; }
[]{ ++global_x; } 
[](int x, int y) -> int { int z = x + y; return z + x; }
```

前面的[]指定变量传递规则：

```c++
[]      // 沒有定义任何变量。使用未定义变量会引发错误。
[x, &y] // x以传值方式传入（默认），y以引用方式传入。
[&]     // 任何被使用到的外部变量都隐式地以引用方式加以引用。
[=]     // 任何被使用到的外部变量都隐式地以传值方式加以引用。
[&, x]  // x显式地以传值方式加以引用。其余变量以引用方式加以引用。
[=, &z] // z显式地以引用方式加以引用。其余变量以传值方式加以引用。
```



# 引用

引用很容易与指针混淆，它们之间有三个主要的不同：

- 不存在空引用。引用必须连接到一块合法的内存。
- 一旦引用被初始化为一个对象，就不能被指向到另一个对象。指针可以在任何时候指向到另一个对象。
- 引用必须在创建时被初始化。指针可以在任何时间被初始化。

# 类和对象

```c++
class Box{
    public:
    	double x;
    	double y;
    	double setx(double x);
};

double Box::setx(double der){
    x = der;
    return x;
}


Box box1;
box1.setx(0.1);
```

**私有成员和受保护成员不能使用直接成员访问运算符(.)来直接访问**



## 成员函数

类的成员函数是指那些把定义和原型写在类定义内部的函数

类成员函数是类的一个成员，它可以操作类的任意对象，可以访问对象中的所有成员

成员函数可以定义在类定义内部，或者单独使用**范围解析运算符 ::** 来定义。

在类定义中定义的成员函数把函数声明为**内联**的，即便没有使用 inline 标识符

```c++
class Box{
    public:
    	double x;
    	double get(){
            return x; 
        }
};
```

```c++
class Box{
    public:
    	double x;
    	double get();
};
double Box::get(){
    return x;
}
```

**在 :: 运算符之前必须使用类名**

## 类访问修饰符

```c++
class Box{
    public:
    //公有成员  : 在程序中类的外部是可访问的。您可以不使用任何成员函数来设置和获取公有变量的值
    protected:
    //受保护成员 : 和私有成员类似，但是受保护成员可以在子类中访问
    private:
    //私有成员: 成员变量或函数在类的外部是不可访问的，甚至是不可查看的。只有类和友元函数可以访问私有成员。
}
```

一个类可以有多个 public、protected 或 private 标记区域

成员和类的默认访问修饰符是 private。



## 继承



三种继承方式：

+ public继承

  基类 public 成员，protected 成员，private 成员的访问属性在派生类中分别变成：public, protected, private

+ protected继承

  基类 public 成员，protected 成员，private 成员的访问属性在派生类中分别变成：protected, protected, private

+ private继承

  基类 public 成员，protected 成员，private 成员的访问属性在派生类中分别变成：private, private, private



> private 成员只能被本类成员（类内）和友元访问，不能被派生类访问；
>
> protected 成员可以被派生类访问。

```c++
class A{
    public:
    	int x;
   		int y;
};
class B:public A{
    
};
class C:protected A{
    
};
```

## 构造函数

类的**构造函数**是类的一种特殊的成员函数，它会在每次创建类的新对象时执行

构造函数的名称与类的名称是完全相同的，并且不会返回任何类型，也不会返回 void

