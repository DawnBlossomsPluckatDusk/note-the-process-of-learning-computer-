[TOC]

# 1. C语言入门

> 汇编语言和机器语言的指令是一一对应的  

C标准规定的转义字符

| `\'` | 单引号                                                       |
| ---- | ------------------------------------------------------------ |
| `\"` | 双引号                                                       |
| `\\` | 反斜线                                                       |
| `\?` | 问号                                                         |
| `\a` | 响铃                                                         |
| `\b` | 退格                                                         |
| `\f` | 分页符；主要用于控制打印机在打印源代码时提前分页，这样可以避免一个函数跨两页打印 |
| `\n` | 换行符                                                       |
| `\r` | 回车                                                         |
| `\t` | 水平制表符                                                   |
| `\v` | 垂直制表符                                                   |



**如果在字符串字面值中要表示单引号`'`和问号`?`，既可以使用转义序列`\'`和`\?`，也可以直接用字符`'`,`?`**

**`"`表示字符串的Delimiter而不表示它的字面含义**  



`C99`规定的关键字：

`int`、`char`、`long`、`float`、`double`、`void`、`short`、`signed`、`unsigned`、`__Bool`、`_Complex`、`const`、`enum`

`if`、`else`、`goto`、`for`、`while`、`switch`、`case`、`break`、`continue`、`do`

`auto`、`default`、`extem`、`inline`、`register`、`return`、`sizeof`、`static`、`struct`、`typedef`、`union`、`volatile`、`_Imaginary`

**避免使用下划线开头的标识符，这些标识符往往用作一些扩展功能**

定义（Definition） 和声明（Declaration） 之间的关系是：**如果一个声明同时也要求分配存储空间，则称为定义**  



