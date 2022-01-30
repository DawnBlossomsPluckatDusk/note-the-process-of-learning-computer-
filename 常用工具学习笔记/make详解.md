# make详解

[TOC]



## make介绍

`make`是一个命令工具，用于解释`Makefile`中指令的命令工具。

`Makefile`则是定义了源文件编译顺序，时间等规则的文件，使用`Makefile`可以实现大量源文件的自动编译

**`make`中最重要的就是`Makefile`的编写**

PS：在Linux中常常使用到，Windows集成到了IDE中

## `Makefile`简单介绍

```makefile
target ...: prerequisites ...
command
...
...
```

`target`：目标文件，可以是中间文件，也可以是执行文件

`prerequisites`：生成目标文件需要的文件或目标

`command`：make执行的命令

当`prerequisites`中如果有一个以上的文件比`target`文件要新的话，`command`所定义的命令就会被执行

### 实例：

```makefile
edit: main.o kbd.o command.o display.o \
insert.o search.o files.o utils.o
cc -o edit main.o kbd.o command.o display.o \
insert.o search.o files.o utils.o

main.o: main.c defs.h
cc -c main.c
kbd.o:kbd.c defs.h command.h
cc -c hbd.c
command.o:command.c defs.h command.h
cc -c command.c
display.o: display.c defs.h buffer.h
cc -c display.c
insert.o:insert.c defs.h buffer.h
cc -c insert.c
search.o:search.c defs.h buffer.h
cc -c search.c
files.o:files.c defs.h buffer.h command.h
cc -c files.c
utils.o: utils.c defs.h
cc -c utils.c
clean:
rm edit main.o kbd.o command.o display.o \
insert.o search.o utils.o
```

`\`：换行符

在该目录下输入命令`make`，通过该例子可以生成可执行文件`edit`

如果需要删除中间文件则只需要输入`make clean`命令

特此说明：

`clean`不是一个文件，而是作为一个`lable`，没有依赖文件。所以在`make`命令后需要明显指出这个`lable`的名字。

**`make`会将第一个目标文件作为最终的目标文件**

## `make`中的变量

```makefile
edit: main.o kbd.o command.o display.o \
insert.o search.o files.o utils.o
cc -o edit main.o kbd.o command.o display.o \
insert.o search.o files.o utils.o
```

该处存在重复，可以使用变量解决

```makefile
objects = main.o kbd.o command.o display.o \
insert.o search.o files.o utils.o
edit: $(objects)
cc -o edit $(objects)
```

如果增加`.o`文件，只需要在`objects`中添加



## `make`自动推导功能

GNU的`make`存在自动推导功能，可以自动推导文件以及文件依赖后面的命令。

即：只要`make`看见一个`.o`文件，就会自动把对应的`.c`文件加在依赖关系中

所以上诉例子的`makefile`改进为：

```makefile
objects = main.o kbd.o command.o display.o \
insert.o search.o files.o utils.o

edit: $(objects)
cc -o edit $(objects)

main.o:  defs.h
kbd.o: defs.h command.h
command.o:defs.h command.h
display.o:defs.h buffer.h
insert.o:defs.h buffer.h
search.o:defs.h buffer.h
files.o:defs.h buffer.h command.h
utils.o:defs.h

.PHONY:clean
clean:
rm edit $(objects)
```



`.PHONY`表示`clean`是个伪目标文件



## `Makefile`文件

### 文件总述

Makefile主要包含五个部分：

+ 显式规则
+ 隐晦规则
+ 变量的定义
+ 文件指示
+ 注释

**在Makefile中的命令，必须要以[Tab]键开始**

### 命名

默认的情况下，make命令会在当前目录下按顺序找寻文件名为“GNUmakefile”、“makefile”、“Makefile”的文件。**建议使用`Makefile`**

如果要指定特定的Makefile，你可以使用make的“-f”和“--file”参数，如：make -f Make.Linux或make --file Make.AIX

### 引用其他的`Makefile`

可以通过使用`include`关键字把别的`Makefile`包含进来，被包含的文件会被原模原样放置在当前文的包含位置。

语法格式：

`include <filename>`

`filename`可以包含路径和通配符



如果在`make`执行时，有`-I`或`--include-dir`参数，那么`make`就会在这个参数所指定的目录下去寻找



在`include`的文件中，如果存在没有找到的文件，那么`make`会生成一条警告信息，但不会报错。在完成makefile的读取后，make会再重新尝试寻找这些文件，如果还是不行，那么会出现一条报错

如果希望忽视这些文件，那么需要在`include`前面加上`-`

例如：

```makefile
-include <filename>
```

### GNU中的工作方式

1. 读入所有的Makefile
2. 读入被include的Makefile
3. 初始化文件中的变量
4. 推导隐晦规则
5. 为所有的目标文件创建依赖关系链
6. 根据依赖关系决定哪些目标需要重新生成
7. 执行生成命令

## 文件搜寻

在Makefile中设置特殊变量`VPATH`可以实现让make去寻找依赖的文件

```makefile
VPATH = src:../headers
```

make在当前目录没有找到对应的依赖文件时，会按照这个顺序进行搜寻



另一种搜寻方式：使用`vpath`关键字

1. `vpath <pattern> <directories>`为符合模式<pattern>的文件指定搜索目录<directories>
2. `vpath <pattern>`清除符合模式<pattern>的文件的搜索目录
3. `vpath` 清除所有已被设置好了的文件搜索目录

PS: vpath使用方法中的<pattern>需要包含`%`字符

```makefile
vpath %.h ../headers
```

该语句表示，在../headers目录下搜索所有以.h结尾的文件



如果连续的vpath语句中出现相同的pattern，或是被重复的pattern，那么make会按照vpath语句的先后顺序来执行



## 伪目标

可以使用`.PHONY`来指明一个目标是伪目标

```makefile
.PHONY : clean  #声明clean是伪目标
clean:
rm -rf *.ob
```



## 静态模式

静态模式可以更加容易地定义多目标的规则

语法：

```makefile
<targets> : <target-pattern> : <prereq-patterns>
<command>
```



`target-pattern`：指明了targets的模式

`prereq-patterns`：指定目标的依赖模式，对`target-pattern`形成的模式再进行一次以依赖目标的定义



例子：

如果<target-parrtern>定义成“%.o”，意思是我们的<target>集合中都是以“.o”结尾的，而如果我们的<prereq-parrterns>定义成“%.c”，意思是对<target-parrtern>所形成的目标集进行二次定义，其计算方法是，取<target-parrtern>模式中的“%”（也就是去掉了[.o]这个结尾），并为其加上[.c]这个结尾，形成的新集合



## 显示命令

当我们用`@`字符在命令行前，那么，这个命令将不被make显示出来

```makefile
@echo compiling
```

会输出：compiling



如果make执行时，带入参数`-n`或`--just-print`，那么其只是显示命令，但不会执行命令
