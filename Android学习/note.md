[TOC]

# `Android`学习

## 参考书籍

[第一行代码`Android`](https://book.douban.com/subject/34996842/)

## 笔记结构

按照参考书籍的讲解顺序进行记录

# 第1章---`Android`基础知识

## `Android`的系统架构

大致分为`4`层架构：`Linux`内核层、系统运行层、应用框架层和应用层

+ 内核层：提供底层驱动，如：显示驱动、音频驱动

+ 系统运行层：通过`C/C++`库为`Android`提供主要的特性支持；同时，这一层还拥有`Android`运行时库，主要提供核心库，运行开发者使用`java`语言编写`Android`应用

  > 在运行时库中还包含`Dalvik`虚拟机，在`5.0`之后改为`ART`运行环境。
  >
  > `Dalvik`和`ART`都是专门为移动设备定制的，针对手机内存、CPU性能有限等情况做出了优化处理

+ 应用框架层：主要提供了构建应用程序时可能用到的各种`API`

+ 应用层：所有安装在手机上的应用程序

## `Android`的四大组件

+ `Activity`：负责应用程序的门面，展示应用界面
+ `Service`：在后台运行，即使用户退出了应用，也可以继续运行
+ `BroadcastReceiver`：负责应用接受来自各处的广播消息
+ `ContentProvider`：负责应用程序之间的数据共享

> 在`Android studio`中新建的项目默认采用`Android`模式的项目结构，这个并不是项目的真实结构

## 项目结构介绍 -- 使用`Android studio`

+ `.gradle`和`.idea`：自动生成的文件
+ `app`：项目中的代码、资源等内容都是在该目录下
+ `build`：主要包含编译时自动生成的文件
+ `gradle`：包含了`gradle wrapper`的配置文件
+ `.gitignore`：将指定的目录或文件排除在版本控制之外
+ `build.gradle`：项目全局的`gradle`构建脚本
+ `gradle.properties`：全局的`gradle`配置文件
+ `gradlew`和`gradlew.bat`：用于在命令行中执行`gradle`命令
+ `local.properties`：用于指定本机中的`Android SDK`的位置
+ `settings.gradle`：用于指定项目中所有引入的模块

在`app`目录下的结构

+ `build`：编译时自动生成的文件
+ `libs`：存放第三方的`jar`包，会自动添加到项目的构建路径中
+ `androidTest`：用来编写测试用例
+ `java`：存放代码
+ `res`：存放项目中使用到的图片、布局、字符串等资源
+ `AndroidManifest.xml`：整个`Android`项目的配置文件，四大组件需要在这里注册，同时还能够添加权限声明
+ `test`：用来编写单元测试
+ `build.gradle`：指定项目构建相关配置
+ `proguard-rules.pro`：指定项目代码混淆规则

**没有在`AndroidManifest.xml`里注册的`Activity`不能使用**



`AppCompatActivity`是`AndroidX`中提供的一种向下兼容的`Activity`，可以使`Activity`在不同系统版本中的功能保持一致性

> `Activity`类是`Android`系统提供的一个基类，项目中所有自定义的`Activity`都必须继承它或者它的子类



在`res`目录下

+ 以`drawable`开头的目录用来存放图片的

+ 以`mipmap`开头的目录用来存放应用图标
+ 以`values`开头的目录用于存放字符串、样式、颜色等配置
+ 以`layout`开头的目录用于存放布局文件

对于下面定义的字符串

```xml
<resources>
	<string name = "app_name">HelloWorld</string>
</resources>
```

有两种方式引用它：

+ 在代码中，通过`R.string.app_name`可以获得该字符串
+ 在`XML`中，通过`@string/app_name`可以获得该字符串



> `android:icon`用于指定应用图标
>
> `android:label`用于指定应用名称

## `Gradle`项目构建工具

>  Gradle是一个非常先进的项目构建工具，它使用了一种基于Groovy的领域特定语言（DSL）来进行项目设置，摒弃了传统基于XML（如Ant和Maven）的各种烦琐配置。  

**Gradle并不是专门为构建Android项目而开发的，Java、C++等很多种项目也可以使用Gradle来构建**  

常见语法：

`dependencies`：描述依赖，声明插件名和版本信息

`apply plugin`：应用插件，后接插件名

`compileSdkVersion`：指定项目的编译版本

`buildToolsVersion`：指定项目构建工具的版本

`defaultConfig`：在这里面可以对项目的细节进行配置，**`applicationId`是每一个应用的唯一标识符，不能重复**

`minSdkVersion`：指定项目最低兼容的`Android`系统版本

`targetSdkVersion`：表示在该版本充分测试，系统会启用一些新的功能和特性

`versionCode`：指定项目的版本号

`versionName`：指定项目名

`testInstrumentationRunner`：用于在当前项目启用`JUnit`测试

`buildTypes`：用于指定生成安装文件的相关配置



## `Android`日志工具`Log`

`Android`中的日志工具类是`Log（android.util.Log） `

+ `Log.v()`：用于打印琐碎的日志。级别是`verbose`
+ `Log.d()`：用于打印调试信息。级别是`debug`
+ `Log.i()`：用于打印重要数据。级别是`info`
+ `Log.w()`：用于打印警告信息。级别是`warn`
+ `Log.e()`：用于打印错误信息。级别是`error`

# 第2章---`Kotlin`学习

## 变量

`Kotlin`中只允许在变量前声明两种关键字：`val`和`var`

`val`用来表明一个不可变的变量，**初始化后不能重新赋值**

`var`用来表明一个可变的变量  

> **`Kotlin`每行代码的结尾不用加上分号**

```kotlin
fun main(){
    val a = 10
    var b = 10
    a = 20 //语法错误
    b = 20
}
```

`Kotlin`的类型推导机制可以推导出变量的数据类型，如果延迟赋值则不能自动推导

```kotlin
fun main(){
    var b
    b = 10 //在这里变量b不会被推导出整型
}
```

`Kotlin`完全抛弃了`java`中的基本数据类型，全部使用了对象数据类型

```kotlin
fun main(){
    var b:Int //显示地声明变量地数据类型
    b = 10
}
```

`Kotlin`中的对象数据类型

| 对象数据类型 | 说明         |
| ------------ | ------------ |
| Int          | 整型         |
| Long         | 长整型       |
| Short        | 短整型       |
| Float        | 单精度浮点数 |
| Double       | 双精度浮点数 |
| Boolean      | 布尔型       |
| Char         | 字符型       |
| Byte         | 字节型       |



> 优先使用`val`来声明一个变量，而当`val`没有办法满足你的需求时再使用`var`  

## 函数

定义函数的语法规则

```kotlin
fun methodname(param1:Int,param2:Int):Int{
    return 0
}
```

参数的声明格式是“参数名: 参数类型”  。如果不想接收任何参数，那么写一对空括号就可以了。  

**参数括号后面的那部分是可选的，用于声明该函数会返回什么类型的数据**，上述示例就表示该函数会返回一个Int类型的数据。如果你的函数不需要返回任何数据，这部分可以直接不写。  

<span id="function">语法糖</span>：

> 当一个函数中只有一行代码时，`Kotlin`允许我们不必编写函数体，可以直接将唯一的一行代码写在函数定义的尾部，中间用等号连接即可。  

```kotlin
fun maxa(num1:Int,num2:Int): Int = max(num1,num2)
```

## 条件语句

`Kotlin`中的条件语句主要有两种实现方式：`if`和`when ` 

使用`if`语句实现分支结构

```kotlin
fun maxa(num1:Int,num2:Int):Int{
    if(num1 > num2){
        return num1
    }else{
        return num2
    }
}
```

`Kotlin`中的`if`语句有一个额外的功能，它是可以有返回值的，**返回值就是`if`语句每一个条件中最后一行代码的返回值**

结合之前说过的[语法糖](#function)，代码可以修改为

```kotlin
fun maxa(num1:Int,num2:Int) = if (num1 > num2) num1 else num2
```

> 这里利用了`kotlin`的自动推导机制

### `when`语句



# 第3章

# 第4章

# 第5章

# 第6章

# 第7章

# 第8章

# 第9章

# 第10章

# 第11章

# 第12章

# 第13章

# 第14章

# 第15章

# 第16章

