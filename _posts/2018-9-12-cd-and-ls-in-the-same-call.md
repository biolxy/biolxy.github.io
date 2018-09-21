---
layout: post
title: "将 cd 和 ls 两个命令组合起来使用"
categories: Linux
tags: Linux cd ls
description: 组合cd 和 ls 命令
author: biolxy
mathjax: true
---

* content
{:toc}



## 1、 将 cd 和 ls 两个命令组合起来使用

>  效果：很明显，可以少敲一次命令

### 1.1、 具体实现

编辑 `.bashrc`文件（如果你用的时`zsh`或者时`csh` 就去编辑其对应的配置文件，如`~/.zshrc` 或`~/.cshrc` ）

在文件中添加如下的一个函数`cl`

```shell
cl() {
    cd "$@" && ls
}

alias 'cd'=' cl'
```

保存退出，刷新

```shell
source ~/.bashrc 
source ~/.zshrc
source ~/.cshrc
```

在终端里敲 `cl /home` 查看效果

> 该命令是 cd 和 ls 的组合命令，若cd命令执行失败，则 ls 命令不会执行

