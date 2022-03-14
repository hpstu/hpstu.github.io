---
title: git
top: false
cover: false
toc: true
mathjax: true
date: 2022-03-10 21:40:07
password:
summary:
tags:
categories:
keywords:
description:
---

## 安装好Git后必须要设置的一步（设置用户名和邮箱）：

```sh
git config --global user.name hpstu  						 # --global为可选项
git config --global user.email 571091694@qq.com
#查看配置
git config --global --list                                  # --global可选
git config --system --list
```

## 创建项目

```sh
#在当前目录新建一个Git代码库
git init
# or
git clone [url]
```

## 查看文件状态

```sh
# 查看指定文件状态
git status [filename]
# 查看所有文件状态
git status
# 添加所有文件到暂存区
git add .
# 提交暂存区的内容到本地仓库，-m后面可以跟提交信息
git commit -m "提交的信息"
```

## 文件.gitignore

有些时候我们不想把某些文件纳入版本控制中，比如数据库文件，临时文件，设计文件等。
在主目录下建立”.gitignore”文件，此文件有如下规则：

1. \# 表示注释
2. 可以使用Linux通配符。例如：星号（*）代表任意多个字符，问号（？）代表一个字符，方括号（[abc]）代表可选字符范围，大括号（{string1，string2.…}）代表可选的字符串等。
3. 如果名称的最前面有一个感叹号（！），表示例外规则，将不被忽略。
4. 如果名称的最前面是一个路径分隔符（/），表示要忽略的文件在此目录下，而子目录中的文件不忽略。
5. 如果名称的最后面是一个路径分隔符（/），表示要忽略的是此目录下该名称的子目录，而非文件（默认文件或目录都忽略）。

## git分支

```sh
# 列出所有本地分支
git branch
# 列出所有远程分支
git branch -r
# 新建一个分支，但依然停留在当前分支
git branch [branch-name]
# 新建一个分支，并切换到该分支
git checkout -b [branch-name]
# 合并指定分支到当前分支
git merge [branch-name]
# 删除分支
git branch -d [branch-name]
# 删除远程分支
git push origin --delete [branch-name]
git branch -dr [remote/branch]
```

