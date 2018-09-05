---
layout: post
title: "R 语言的使用"
categories: R
tags: R R包安装
description: R 语言的使用
author: biolxy
mathjax: true
---

* content
{:toc}



### R 拓展包的安装

#### 官方在线安装
例如：安装 "ggplots" 包
```R
options(CRAN="http://cran.r-project.org");
install.packages("ggplots");
```

#### 本地安装

将安装包下载到本地，然后本地安装

我一般都在这俩地址下载：

- https://mirrors.tuna.tsinghua.edu.cn/CRAN/
- http://bioconductor.org/

注意win下的地址的写法

```R
install.packages("C:\\ggplot2.zip",contriburl=NULL)
```

#### 通过第三方工具在线安装（bioconductor）

```R
source("http://bioconductor.org/biocLite.R")
biocLite("ggplots")
```

#### 通过第三方工具在线安装（devtools + github）安装发布在github上的R包

例如：安装 "clonevol" 包，安装GitHub上的R包，需要指定R包的作者，一般这个包的github页面都会介绍安装的步骤  

```R
install.packages('devtools')
library(devtools)
install_github('hdng/clonevol')
```

### 内网安装

https://blog.csdn.net/liu365560704/article/details/70321153

### 常用技巧

```R
rm(list = ls(all=TRUE))   # 清除变量
```



