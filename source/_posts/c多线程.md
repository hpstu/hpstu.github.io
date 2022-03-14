---
title: c++多线程
top: false
cover: false
toc: true
mathjax: true
date: 2022-03-12 15:17:55
password:
summary:
tags:
categories:
keywords:
description:
---

# C++多线程

引入头文件：

```c++
#include <thread>  
```

链接库：

```cmake
target_link_libraries(xxx pthread)
```
**线程**
• 进程可以包含多个线程
• 主线程：从main函数开始，main函数执行完，主线程结束，进程结束
• 其他线程：需要我们自己创建，入口可以是函数、类、lambda表达式
• 进程是否执行完毕的标志是：主线程是否执行完，如果主线程执行完毕了，就代表整个进程执行完
了，一般来说，此时如果其他子线程还没有执行完，也会被强行终止

## 创建子线程

```c++
std::thread	// 多线程类
```

```c++
int main(){
    
    thrad th1(xx);		// 使用其创建子线程对象的时候，子线程就直接运行了。
    ...
	th1.join();			// 汇合，表示等待子线程运行完，主线程再接着运行。（小溪汇入大河）
}
```

```c++
int main(){
    
    thrad th2(xx);		// 使用其创建子线程对象的时候，子线程就直接运行了。
    ...
	th2.detach();		// 分离，表示子线程脱离子线程，各走各的，互不相关（分流了），注意，此时如果主线程运行完毕，则直接关闭所有线程，子线程没运行完也停止运行。（夭折了），少用。
    
    th2.joinable()		// joinable()判断是否可以成功使用join()或者detach(),如果返回true，证明可以调用join()或者detach(),如果返回false，证明调用过join()或者detach()，join()和detach()都不能再调用了
}

```

查看线程ID:

```c++
 this_thread::get_id()	//  this_thread是一个namespace,
```
## 进程间安全（防止读写冲突，就给它上锁）
```c++
#include <mutex>  // 互斥锁头文件 ，mutex也是一个对象，相当于一把锁。
```

使用方法：

先创建一个锁对象：

```c++
mutex myMutex;
```

上锁是防止多个线程**同时**对**同一个数据快**进行读写，造成冲突，（也就是前一个线程刚写入一半数据，你就过来读了），正确姿势是等他写完你再读，或读完你再写。所以，当其中一个线程拿到一把锁时：

```c++
// 线程1
myMutex.lock();			
cout << "putInData 子线程：放入一个数据" << i << endl;
dataQuene.push_back(i);
myMutex.unlock();
```

```C++
// 线程2
myMutex.lock();			
cout << "takeOutData 子线程：取出一个数据" << dataQuene.front() << endl;
dataQuene.pop_front();
myMutex.unlock();
```

这两个线程为互斥关系，各自当执行到`myMutex.lock()`时，必须等一个拿到锁的代码快完整执行到释放锁`myMutex.unlock()`了，另一个才可能拿到锁继续执行。

> 注意：`lock()`与`unlock()`必须成对出现

**lock_guard**

- 内部构造时相当于执行了lock，析构时相当于执行unlock
- 简单但不如lock()和unlock()灵活，通过大括号来实现，控制生命周期

```c++
mutex myMutex;

{
    lock_guard<mutex> dataGuard(myMutex);		// lock_guard<mutex>创建对象，参数为锁对象
	...
    ...    
}
```

**unique_lock**

std::unique_lock要比std::lock_guard功能更多，有更多的成员函数，更灵活,但是更灵活的代价是占用空间相对更大一点且相对更慢一点

```
mutex myMutex;

{
    unique_lock<mutex> dataOutUnique(myMutex);	// unique_lock<mutex>创建对象，参数为锁对象
	...
    ...    
}
```

**死锁产生原因及防止方法**

当两个线程同时拿着两把及以上的相同的锁时，上锁的顺序必须要一致，否则会发生死锁。