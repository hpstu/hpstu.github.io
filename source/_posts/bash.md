---
title: bash
top: false
cover: false
toc: true
mathjax: false
date: 2022-03-06 12:16:37
password:
summary:
tags: bash 
categories:
keywords:
description:
---

# bash 执行方式
脚本执行方式：
1. 直接输入文件及其路径（绝对路径或者相对路径）
```bash
./haha.sh   
/home/hp/桌面/haha.sh
```
2. 也可以将脚本文件作为参数传给Shell程序，使其解释并执行脚本文件的·内容
```sh
bash ./haha.sh
```
3. 还可以使用source命令执行文件
```sh
source ./haha.sh
```
如果文件没权限执行则添加权限
```sh
sudo chmod +x ./haha.sh
```
**注意变量赋值等号( = )之间不能有空格**
```sh
n=10         #True
n = 10       #False
```
Shell是一种弱类型的编程语言。(类似Python)

## 重定向

与 Unix 主题“任何东西都是一个文件”保持一致，程序，比方说 ls，实际上把他们的运行
结果输送到一个叫做标准输出的特殊文件（经常用 stdout 表示），而它们的状态信息则送到另
一个叫做标准错误的文件（stderr）。默认情况下，标准输出和标准错误都连接到屏幕，而不是
保存到磁盘文件。除此之外，许多程序从一个叫做标准输入（stdin）的设备得到输入，默认情
况下，标准输入连接到键盘。

I/O 重定向允许我们更改输出地点和输入来源。一般地，输出送到屏幕，输入来自键盘，但
是通过 I/O 重定向，我们可以做出改变。

I/O 重定向允许我们来重定义标准输出的地点。我们使用 “>” 重定向符后接文件名将标准输
出重定向到除屏幕以外的另一个文件。为什么我们要这样做呢？因为有时候把一个命令的运
行结果存储到一个文件很有用处。例如，我们可以告诉 shell 把 ls 命令的运行结果输送到文件
ls-output.txt 中去，由文件代替屏幕。

```bash
ls -l /usr/bin > ls-output.txt
```

这里，我们创建了一个长长的目录/usr/bin 列表，并且输送程序运行结果到文件 ls-output.txt
中。

程序不把它的错误信息输送到标准输出。反而，像许多写得不错的 Unix 程序，ls 把错误信息送到标准错误。



## 命令的组合符`&&`和`||`

除了分号，Bash 还提供两个命令组合符`&&`和`||`，允许更好地控制多个命令之间的继发关系。

```sh
Command1 && Command2
```

上面命令的意思是，如果`Command1`命令运行成功，则继续运行`Command2`命令。

```sh
Command1 || Command2
```

上面命令的意思是，如果`Command1`命令运行失败，则继续运行`Command2`命令。

## 快捷键

Bash 提供很多快捷键，可以大大方便操作。下面是一些最常用的快捷键，完整的介绍参见《行操作》一章。

- `Ctrl + L`：清除屏幕并将当前行移到页面顶部。
- `Ctrl + C`：中止当前正在执行的命令。
- `Shift + PageUp`：向上滚动。
- `Shift + PageDown`：向下滚动。
- `Ctrl + U`：从光标位置删除到行首。
- `Ctrl + K`：从光标位置删除到行尾。
- `Ctrl + D`：关闭 Shell 会话。
- `↑`，`↓`：浏览已执行命令的历史记录
- ctrl+A 跳到光标所在行的行首
- ctrl+E 跳到光标所在行的行尾
- ctrl+W 删除光标前的单个域

### Bash 只有一种数据类型，就是字符串。不管用户输入什么数据，Bash 都视为字符串。



## 变量声明的语法如下。

```sh
variable=value
```

上面命令中，等号左边是变量名，右边是变量。注意，等号两边不能有空格。

如果变量的值包含空格，则必须将值放在引号中。

```sh
myvar="hello world"
```



## Shebang 行 

脚本的第一行通常是指定解释器，即这个脚本必须通过什么解释器执行。这一行以`#!`字符开头，这个字符称为 Shebang，所以这一行就叫做 Shebang 行。

`#!`后面就是脚本解释器的位置，Bash 脚本的解释器一般是`/bin/sh`或`/bin/bash`。

```sh
#!/bin/sh
# 或者
#!/bin/bash
```

`#!`与脚本解释器之间有没有空格，都是可以的。

如果 Bash 解释器不放在目录`/bin`，脚本就无法执行了。为了保险，可以写成下面这样。

```sh
#!/usr/bin/env bash
```

上面命令使用`env`命令（这个命令总是在`/usr/bin`目录），返回 Bash 可执行文件的位置。`env`命令的详细介绍，请看后文。

Shebang 行不是必需的，但是建议加上这行。如果缺少该行，就需要手动将脚本传给解释器。举例来说，脚本是`script.sh`，有 Shebang 行的时候，可以直接调用执行。

```sh
$ ./script.sh
```

上面例子中，`script.sh`是脚本文件名。脚本通常使用`.sh`后缀名，不过这不是必需的。

如果没有 Shebang 行，就只能手动将脚本传给解释器来执行。

```sh
$ /bin/sh ./script.sh
# 或者
$ bash ./script.sh
```

```sh
# 给所有用户执行权限
$ chmod +x script.sh

# 给所有用户读权限和执行权限
$ chmod +rx script.sh
# 或者
$ chmod 755 script.sh
```

脚本的权限通常设为`755`（拥有者有所有权限，其他人有读和执行权限）或者`700`（只有拥有者可以执行）。

建议在主目录新建一个`~/bin`子目录，专门存放可执行脚本，然后把`~/bin`加入`$PATH`。

```sh
export PATH=$PATH:~/bin
# “ ：“(冒号)是 PATH 的分隔符（上面相当于赋值操作了）
```

上面命令改变环境变量`$PATH`，将`~/bin`添加到`$PATH`的末尾。可以将这一行加到`~/.bashrc`文件里面，然后重新加载一次`.bashrc`，这个配置就可以生效了。

```sh
$ source ~/.bashrc
```

以后不管在什么目录，直接输入脚本文件名，脚本就会执行。

```sh
$ script.sh
```

上面命令没有指定脚本路径，因为`script.sh`在`$PATH`指定的目录中。

## source 命令

`source`命令用于执行一个脚本，通常用于重新加载一个配置文件。

```sh
$ source .bashrc
```

`source`命令最大的特点是在当前 Shell 执行脚本，不像直接执行脚本时，会新建一个子 Shell。所以，`source`命令执行脚本时，不需要`export`变量。

```sh
#!/bin/bash
# test.sh
echo $foo
```

上面脚本输出`$foo`变量的值。

```sh
# 当前 Shell 新建一个变量 foo
$ foo=1

# 打印输出 1
$ source test.sh
1

# 打印输出空字符串
$ bash test.sh
```

上面例子中，当前 Shell 的变量`foo`并没有`export`，所以直接执行无法读取，但是`source`执行可以读取。

`source`命令的另一个用途，是在脚本内部加载外部库。

```sh
#!/bin/bash

source ./lib.sh

function_from_lib
```

上面脚本在内部使用`source`命令加载了一个外部库，然后就可以在脚本里面，使用这个外部库定义的函数。

`source`有一个简写形式，可以使用一个点（`.`）来表示。

```sh
$ . .bashrc
```
