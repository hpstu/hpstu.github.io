---
title: C++内置库以及函数
top: false
cover: false
toc: true
mathjax: true
date: 2022-03-15 13:32:14
password:
summary:
tags:
categories:
- C++
- 内置库和函数
keywords:
description:
---

# C++内置库以及函数

## sizeof

1. **定义**

> sizeof是一个操作符（operator）。
>
> 其作用是返回一个对象或类型所占的**内存字节数**。

2. **语法**

> sizeof有三种语法形式：
>
>``` c++
>1） sizeof (object); //sizeof (对象)
>2） sizeof object;  //sizeof 对象
> 3） sizeof (type_name); //sizeof (类型)
>```
> 对象可以是各种类型的变量，以及表达式（一般sizeof不会对表达式进行计算）。
>
> sizeof对对象求内存大小，最终都是转换为对对象的数据类型进行求值。
>
> sizeof (表达式); //值为表达式的最终结果的数据类型的大小

- **基本数据类型的sizeof**

  - 这里的基本数据类型是指short、int、long、float、double这样的简单内置数据类型。

    由于它们的内存大小是和系统相关的，所以在不同的系统下取值可能不同。

```c++
int i;  
sizeof(int); //值为4  
sizeof(i); //值为4，等价于sizeof(int)  
sizeof i; //值为4  
sizeof(2); //值为4，等价于sizeof(int)，因为2的类型为int  
sizeof(2 + 3.14); //值为8，等价于sizeof(double)，因为此表达式的结果的类型为double  
```



- **结构体的sizeof**

  - 结构体的sizeof涉及到字节对齐问题。

    为什么需要字节对齐？计算机组成原理教导我们这样有助于加快计算机的取数速度，否则就得多花指令周期了。为此，编译器默认会对结构体进行处理（实际上其它地方的数据变量也是如此），让宽度为2的基本数据类型（short等）都位于能被2整除的地址上，让宽度为4的基本数据类型（int等）都位于能被4整除的地址上，依次类推。这样，两个数中间就可能需要加入填充字节，所以整个结构体的sizeof值就增长了。

    注意：空结构体（不含数据成员）的sizeof值为1。试想一个“不占空间“的变量如何被取地址、两个不同的“空结构体”变量又如何得以区分呢，于是，“空结构体”变量也得被存储，这样编译器也就只能为其分配一个字节的空间用于占位了。

```c++
struct S1  
{  
    char a;  
    int b;  
};  
sizeof(S1); //值为8，字节对齐，在char之后会填充3个字节。  
  
struct S2  
{  
};  
sizeof(S2); //值为1，空结构体也占内存
```

- **数组的sizeof**

  数组的sizeof值等于数组所占用的内存字节数。

  注意：1）**当字符数组表示字符串时，其sizeof值将’/0’计算进去。**

  ​           2）**当数组为形参时，其sizeof值相当于指针的sizeof值。** 

```c++
char a[10];  
char n[] = "abc";   
  
cout<<"char a[10] "<<sizeof(a)<<endl;//数组，值为10  
  
cout<<"char n[] = abc "<<sizeof(n)<<endl;//字符串数组，将'/0'计算进去，值为4
```

- **指针的sizeof**

  指针是用来记录另一个对象的地址，所以指针的内存大小当然就等于计算机内部地址总线的宽度。

  在32位计算机中，一个指针变量的返回值必定是4。

  **指针变量的sizeof值与指针所指的对象没有任何关系。**

## C++字符串

C++大大增强了对字符串的支持，除了可以使用C风格的字符串，还可以使用内置的 string 类。string 类处理起字符串来会方便很多，完全可以代替C语言中的字符数组或字符串指针

string 是 C++ 中常用的一个类

使用 string 类需要包含头文件`<string>`，下面的例子介绍了几种定义 string 变量（对象）的方法：

```c++
#include <iostream>
#include <string>
using namespace std;

int main(){
    string s1;
    string s2 = "c plus plus";
    string s3 = s2;
    string s4 (5, 's');
    return 0;
}
```

> 变量 s1 只是定义但没有初始化，编译器会将默认值赋给 s1，默认值是`""`，也即空字符串。
>
> 变量 s2 在定义的同时被初始化为`"c plus plus"`。与C风格的字符串不同，string 的结尾**没有结束标志**`'\0'`。
>
> 变量 s3 在定义的时候直接用 s2 进行初始化，因此 s3 的内容也是`"c plus plus"`。
>
> 变量 s4 被初始化为由 5 个`'s'`字符组成的字符串，也就是`"sssss"`。
>
> 从上面的代码可以看出，string 变量可以直接通过赋值操作符`=`进行赋值。string 变量也可以用C风格的字符串进行赋值，例如，s2 是用一个字符串常量进行初始化的，而 s3 则是通过 s2 变量进行初始化的。

- 转换为C风格的字符串：

  虽然 C++ 提供了 string 类来替代C语言中的字符串，但是在实际编程中，有时候必须要使用C风格的字符串（例如打开文件时的路径），为此，string 类为我们提供了一个转换函数 c_str()，该函数能够将 string 字符串转换为C风格的字符串，并返回该字符串的 const 指针（const char*）。

```c++
string path = "D:\\demo.txt";
FILE* fp = fopen(path.c_str(), "rt"); // FILE是一个结构体，fopen参数为c风格字符串，该函数返回一个指向 FILE 结构的指针。
```

**为了使用C语言中的 fopen() 函数打开文件，必须将 string 字符串转换为C风格的字符串。**