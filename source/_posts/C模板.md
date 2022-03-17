---
title: C++模板
top: false
cover: false
toc: true
mathjax: true
date: 2022-03-16 22:19:21
password:
summary:
tags:
categories: C++
keywords:
description:
---

# C++ 模板

## 函数模板



模板函数与普通函数的区别：

- 函数模板**不允许**自动类型转换 ， 但传入实参时可以自动进行类型推导，不用加`<type >`
- 普通函数能够自动进行类型转换

同名函数优先调用普通函数，除非显式加`< type>`