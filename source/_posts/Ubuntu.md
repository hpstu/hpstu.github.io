---
title: Ubuntu 命令
top: false
cover: false
toc: true
mathjax: false
date: 2022-03-06 10:54:21
password:
summary:
tags: ubuntu
categories: Ubuntu
keywords: ubuntu
description:
---

# Ubuntu 命令

## 1、文件/文件夹管理

- `ls` 列出当前目录下的所有文件（不显示隐藏文件）listq

- `cd` 或者 `cd ~`进入用户主目录 Change Directory

- `cd -`返回进入此目录之前所在的目录

- `mkdir dirname` 新建目录 Make Directory

- `rmdir dirname` 删除空目录

- `rm filename` 删除文件Remove Directory

- `rm -rf dirname` 删除非空目录及其包含的所有文件

- `mv file1 file2`将文件1重命名为文件2

- `mv file1 dir1` 将文件1移动到目录1中

- `find 路径 -name “字符串”` 查找路径所在范围内满足字符串匹配的文件和目录

- `sudo su` 转到管理员权限执行命令

- `pwd`指出当前所在的路径。是print working directory的缩写。

- `cat`  查看ubuntu中文本文件的内容 concatenate

- `cat file1 file2>>file3`  把文件1和文件2的内容联合起来放到 file3中

- `su`   切换用户  switch user

- `ps`  (-auxf) 进程状态，类似于 windows 的任务管理器 process status

- `df`  其功能是显示磁盘可用空间数目信息及空间结点信息 disk free

- `ln -s` 创建一个软链接，相当于创建一个快捷方式 ink -soft 

- `man` 命令手册 manual

- `chown` change owner

- `chgrp` change group

- `chmod`  change mode

- `tar`  tape archive

- 文件结尾的"rc"（如.bashrc、.xinitrc 等）：Resource configuration

- c++ 文件扩展名后缀：
	- .a（扩展名 a）：Archive，static library
	- .so（扩展名 so）：Shared object，dynamically linked library
	- .o（扩展名 o）：Object file，complied result of C/C++ source file
	
- apt：Advanced package tool

- | grep：Global Regular Expression Print， 全局正则表达式版本

- 目录：
	- /bin = BInaries
	- /dev = Devices
	- /etc = Etcetera ; Editable Text Configuration, 可编辑文本配置， 便成了专门放置系统配置文件的目录
	- /lib = LIbrary
	- /proc = Processes
	- /sbin = Superuser Binaries
	- /tmp = Temporary
	- /usr = Unix Shared Resources
	- /var = Variable ?
	
- 
	
	



## 2、程序安装与卸载

- `chmod`   用于改为用户对于文件的操作权限
- `remove` 卸载指定的程序，一般最好加上“--purge”执行清除式卸载；并在程序名称后添加*号。举例：`sudo apt-get remove --purge nvidia*`  卸载 nvidia 的驱动及其配置文件
- `update` 更新本地软件源文件，需要管理员权限，举例：`sudo apt-get update`

## 3、打包/解压

这里需要先解释几个参数。

| 参数 | 含义                       | 参数 | 含义                 |
| :--- | :------------------------- | :--- | :------------------- |
| -c   | 建立压缩档案               | -z   | 有gzip属性的         |
| -t   | 查看内容                   | -j   | 有bz2属性的          |
| -u   | 更新原压缩包中的文件       | -Z   | 有compress属性的     |
| -x   | 解压                       | -v   | 显示所有过程         |
| -r   | 向压缩归档文件末尾追加文件 | -O   | 将文件解开到标准输出 |

上表左边五个参数是独立的命令，压缩解压都要用到其中一个，可以和别的命令连用但只能用其中一个。右边五个参数是根据需要在压缩或解压时可选的。
 下面进行举例说明。
 **压缩**

- `tar -cvf jpg.tar *.jpg` 将目录里所有jpg文件打包成tar.jpg
- `tar -czf jpg.tar.gz *.jpg`   将目录里所有jpg文件打包成jpg.tar后，并且将其用gzip压缩，生成一个gzip压缩过的包，命名为jpg.tar.gz
- `tar -cjf jpg.tar.bz2 *.jpg` 将目录里所有jpg文件打包成jpg.tar后，并且将其用bzip2压缩，生成一个bzip2压缩过的包，命名为jpg.tar.bz2
- `tar -cZf jpg.tar.Z *.jpg`   将目录里所有jpg文件打包成jpg.tar后，并且将其用compress压缩，生成一个umcompress压缩过的包，命名为jpg.tar.Z
- `rar a jpg.rar *.jpg` rar格式的压缩，需要先下载rar for linux
- `zip jpg.zip *.jpg` zip格式的压缩，需要先下载zip for linux

**解压**

- `tar -xvf file.tar` 解压 tar包
- `tar -xzvf file.tar.gz` 解压tar.gz
- `tar -xjvf file.tar.bz2`   解压 tar.bz2
- `tar -xZvf file.tar.Z`   解压tar.Z
- `unrar e file.rar` 解压rar
- `unzip file.zip` 解压zip

**总结**
 .tar 用 tar -xvf 解压
 .gz 用 gzip -d或者gunzip 解压
 .tar.gz和.tgz 用 tar -xzf 解压
 .bz2 用 bzip2 -d或者用bunzip2 解压
 .tar.bz2用tar -xjf 解压
 .Z 用 uncompress 解压
 .tar.Z 用tar -xZf 解压
 .rar 用 unrar e解压
 .zip 用 unzip 解压

## 4、用户管理

- `sudo useradd username` 创建一个新的用户username
- `sudo passwd username` 设置用户username的密码
- `sudo groupadd groupname` 创建一个新的组groupname
- `sudo usermod -g groupname username` 把用户username加入到组groupname中
- `sudo chown username:groupname dirname` 将指定文件的拥有者改为指定的用户或组

## 5、系统管理

- `uname -a` 查看内核版本
- `cat /etc/issue` 查看ubuntu版本
- `sudo fdisk -l` 查看磁盘信息
- `df -h` 查看硬盘剩余空间
- `free -m` 查看当前的内存使用情况
- `ps -A` 查看当前有哪些进程
- `kill 进程号`或者 `killall 进程名` 杀死进程
- `kill -9 进程号` 强制杀死进程



设置 conda代理：

原本是空白文件

```
sudo gedit ~/.condarc
```

在文件中添加代理端口：

```
proxy_servers:
  http: http://127.0.0.1:7890
  https: https://127.0.0.1:7890
```
