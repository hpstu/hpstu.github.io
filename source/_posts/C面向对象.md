---
title: C++面向对象
top: false
cover: false
toc: true
mathjax: true
date: 2022-03-11 10:39:35
password:
summary:
tags:
categories:
keywords:
description:
---

# Object Oriented

## Header(头文件)布局

```c++
#ifndef __COMPLEX__
#define __COMPLEX__

#include <cmath>

class ostream;          // forward declarations
class complex;			// 前置声明

complex&
__doapl (complex* ths, const complex& r);

class complex			// class declarations
{						// 类-声明
...
};
complex::function ...	// class definition
    					// 类-定义

#endif
```

## 构造函数

构造函数不能带返回值，函数名必须与类名一致，可以列表初始化：

```c++
class complex
{
public:
complex (double r = 0, double i = 0)
: re (r), im (i)						// initialization list
{ }
    
// assignments 赋值
 complex (double r = 0, double i = 0)
{ re = r; im = i; }

complex& operator += (const complex&);
double real () const { return re; }
double imag () const { return im; }
private:
double re, im;
friend complex& __doapl (complex*, const complex&); 
};

```

```c++
// 以下两种创建对象方式没有区别
complex c1
complex c2()
// 因此，不能出现以下方式的两种构造函数，会发生歧义    
complex (double r = 0, double i = 0)
: re (r), im (i) 
{ }
complex () : re(0), im(0) { }
```

## const

```c++
double real () const { return re; }
// const 放在这个位置表示他不会改变{}中引进来的数据，也就是不会改变对象中的数据re.(如果你真的不会改变对象中的数据，那你最好一定要加const,防止外部调用时出错)
// eg. 如果real()声明时没加const，下面就会报错
const complex c1(1,3);
cout << c1.real();
```

## pass by reference & return by reference

传递参数尽量都传递引用，如果不想改变该参数，就加`const`修饰。

返回值也尽量传递引用（如果可以的话）。

**参数传递和返回其实相当于赋值（也即复制），以什么方式递出去，以什么方式接收是各自的事情。**

`return  xx`  和实参相当于是传递者，返回值 和形参相当于是接收者**即赋值操作：**（  接收 =  传递）

- 函数的返回值用于初始化在调用函数时创建的临时对象(temporary object)，如果返回类型不是引用，在调用函数的地方会将函数返回值复制给临时对象。

- 当函数返回非引用类型时，其返回值既可以是局部对象

- 千万不要返回局部对象的引用！千万不要返回指向局部对象的指针！

```c++
int& abc(int a, int& result)		// result接收的是该参数的引用。

{
     result = a;
     return result;					// 返回的是值，单以引用方式接收
}
```

## 操作符重载（操作符也相当于是函数）

 所有的成员函数隐藏了一个`this`参数（即对象的地址指针），在声明函数的时候**不能**显式声明，但可以显式调用

# Object  Based

 ## Big Three 三个特殊函数（class with pointer members 必须有copy ctor 和 copy op  )

```c++
class String
{
public:
String(const char* cstr = 0); 
String(const String& str); 				// 1. 拷贝构造函数，参数为相同对象
String& operator=(const String& str); 	// 2. 拷贝赋值函数，参数为相同对象
~String(); 						 // 3. 析构函数，当对象死亡时自动调用它，并不是它让对象死亡
char* get_c_str() const { return m_data; }
private:
char* m_data;
};
```

```c++
Complex* pc = new Complex(1,2);
...
delete pc;
// 编译器转化为：
//Complex::~Complex(pc); // 析构函数
//operator delete(pc); // 释放内存，其内部调用 free(pc)
```



