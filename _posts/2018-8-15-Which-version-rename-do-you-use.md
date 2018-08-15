---
layout: post
title: "你用的是哪个版本的rename"
categories: Linux
tags: Linux rename
description: rename的介绍
author: biolxy
mathjax: true
---

# 你用的是哪个版本的rename

###  mv 与 rename 的区别

>  **“mv命令只能对单个文件重命名”，这实就是mv命令和rename命令的在重命名方面的根本区别。**

当你的目录下由大量的文件需要批量命名时，rename的作用就体现出来了，例如100个后最为`.html` 的文件替换为 `.txt` 文件，通常你需要写一行for循环，如

```shell
$ for i in `ls *.html`;do name=`basename .html`; mv $i $name.txt ;done
```

如果你想替换的字符不再文件的最前或最后方，由或则你想实现更多功能，那么你就需要`rename`

### rename 有两个版本

**Linux** 的rename命令有两个版本，一个是C语言版本的，一个是Perl语言版本的，早期的Linux发行版基本上使用的是C语言版本的，现在已经很难见到C语言版本的了，由于历史原因，在Perl语言大红大紫的时候，Linux的工具开发者们信仰Perl能取代C，所以大部分工具原来是C版本的都被Perl改写了，因为Perl版本的支持正则处理，所以功能更加强大，已经不再需要C语言版本的了。

### 如何区分系统里的rename命令是哪个版本的?

我在网上搜到描述都是这个

> 输入man rename 看到第一行是
> RENAME(1)     Linux Programmer’s Manual      RENAME(1)
> 那么 这个就是C语言版本的。
> 而如果出现的是：
> RENAME(1)              Perl Programmers Reference Guide              RENAME(1)
> 这个就是Perl版本的了！

然而这根本就**不对！**

我的 centOS下是c版rename ：

![mark](http://oiz501hli.bkt.clouddn.com/blog/180815/97ElgiA8d0.jpg?imageslim)

是`RENAME(1)            User Commands              RENAME(1)`

并不是 `RENAME(1)     Linux Programmer’s Manual      RENAME(1)`

我的ubuntu下是perl版的rename：

![mark](http://oiz501hli.bkt.clouddn.com/blog/180815/BIbgEChBhI.jpg?imageslim)

是`RENAME(1p)     User Contributed Perl Documentation          RENAME(1p)`

并不是`RENAME(1)              Perl Programmers Reference Guide              RENAME(1)`

**正确的做法:**

```shell
vim `which rename`
```

c语言版本的是一个二进制文件，打开后是乱码

perl语言的你会明显看到 `#!/usr/bin/perl -w` 

亦或者你可以通过上述图片中rename的用法来判断你用的是哪个版本

### 两个版本的语法差异：

C语言的rename的语法格式是：

```shell
rename .html .txt  *.html
```

C语言的rename的语法格式是：

```shell
rename 's/\.html$/\.txt/' *.html
```

- rename C语言版本所能实现的功能：批量修改文件名，结果是每个文件会被用相同的一个字符串替换掉！也就是说，无法实现诸如循环 然后按编号重命名！
- **重点：** C语言版本的rename无法处理带有`-` 的替换（别问怎么知道，都是泪啊）

### Perl 版本的批量重命名

Perl 版本的好处是，你可以使用正则表达式来完成很奇特的功能。

具体用法就不细说了。

### 如果你的服务器不幸安装的时C版的rename，怎么办？安装一个rename

```
conda install rename
```

