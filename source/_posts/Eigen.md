---
title: Eigen
top: false
cover: false
toc: true
mathjax: true
date: 2022-03-13 20:03:14
password:
summary:
tags:
categories:
keywords:
description:
---

# Eigen



```c++
a = a.transpose(); // !!! do NOT do this !!!
// For in-place transposition, as for instance in a = a.transpose(), simply use the transposeInPlace() function:
a.transposeInPlace();
//  Eigen treats matrix multiplication as a special case and takes care of introducing a temporary here, so it will compile m=m*m as:
// tmp = m*m;
// m = tmp;

```

```C++
#include <iostream>
#include <Eigen/Dense>
 
using namespace std;
int main()
{
  Eigen::Matrix2d mat;
  mat << 1, 2,
         3, 4;
  cout << "Here is mat.sum():       " << mat.sum()       << endl;
  cout << "Here is mat.prod():      " << mat.prod()      << endl;
  cout << "Here is mat.mean():      " << mat.mean()      << endl;
  cout << "Here is mat.minCoeff():  " << mat.minCoeff()  << endl;
  cout << "Here is mat.maxCoeff():  " << mat.maxCoeff()  << endl;
  cout << "Here is mat.trace():     " << mat.trace()     << endl;
}
/*
Here is mat.sum():       10
Here is mat.prod():      24
Here is mat.mean():      2.5
Here is mat.minCoeff():  1
Here is mat.maxCoeff():  4
Here is mat.trace():     5
*/

```

