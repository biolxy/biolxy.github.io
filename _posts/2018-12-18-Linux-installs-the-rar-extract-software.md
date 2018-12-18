---
layout: post
title: "Linux 安装 rar 解压软件"
categories: Linux develop
tags: linux 压缩 解压
description: linux 压缩 解压
author: biolxy
mathjax: true
---

* content
{:toc}

# Linux 安装 rar 解压软件

## 系统环境

centOS7

```shell
[root@dbf69ae824f9 TCR]# uname -m&&uname -r
x86_64
3.10.0-514.21.1.el7.x86_64
[root@dbf69ae824f9 TCR]# cat /etc/redhat-release
CentOS Linux release 7.6.1810 (Core)
```

## 下载软件包

```shell
wget http://www.rarlab.com/rar/rarlinux-3.8.0.tar.gz
tar zxvf rarlinux-3.8.0.tar.gz
cd rar
make
make install 
```

注意：安装需要root权限，没有的话建议用docker尝试，或者自己修改makefile里面的`PREFIX` 路径

## 遭遇bug

在尝试解压文件时：

```shell
rar x example.rar 
```

遇到报错：

```shell
bash: /usr/local/bin/rar: /lib/ld-linux.so.2: bad ELF interpreter: No such file or directory
```

## 解决

- 解释：是因为64位系统中安装了32位程序

- 解决方法：

```shell
yum install glibc.i686
```

再次尝试解压文件，又遇到报错：

```shell
error while loading shared libraries: libstdc++.so.6: cannot open shared object file: No such file or directory
```

- 解决办法：

```shell
[root@dbf69ae824f9 rar]# yum whatprovides libstdc++.so.6
Loaded plugins: fastestmirror, ovl
Loading mirror speeds from cached hostfile
 * base: mirrors.shu.edu.cn
 * extras: mirrors.163.com
 * updates: mirrors.163.com
libstdc++-4.8.5-36.el7.i686 : GNU Standard C++ Library
Repo        : base
Matched from:
Provides    : libstdc++.so.6



libstdc++-4.8.5-36.el7.i686 : GNU Standard C++ Library
Repo        : @base
Matched from:
Provides    : libstdc++.so.6



[root@dbf69ae824f9 rar]#
```

` yum whatprovides libstdc++.so.6`该命令会提示哪个安装包有这个库文件如上，`libstdc++-4.8.5-36.el7.i686` 这个包可以提供 `libstdc++.so.6`库，执行

```shell
yum install libstdc++-4.8.5-36.el7.i686
```

然后，`rar` 就可以用了

`rar -h` 查看使用帮助。

## 参考

- https://haiwei2009.iteye.com/blog/1908263
- http://blog.51cto.com/oldboy/597515