---
layout: post
title: "基于已有的image创建dockerimages"
categories:
  -  Docker 
tags: 
  - Linux
  - Docker
description: 基于已有的image，创建docker images
authors: [biolxy]
mathjax: true
date: 2018-8-19
---


{:toc}






### 1、 创建docker images 有两种方法

- 运行一个容器，在运行容器的基础上安装软件，部署服务，然后导出为新的image
- 通过编写dockerfile文件来创建新的image

>  配合docker 私有仓库，可以用来部署生产环境

### 2、 缘起

本文记录了在一个已近存在的docker image，安装软件，并保存为新image的过程。

首先通过：

```shell
docker images
```

找到我们事先下载好得

```shell
docker images
```

`docker.io/hiono/texlive`

### 3、 运行 docker 镜像：

```shell
docker run -it docker.io/hiono/texlive 
```

**以下操作都发生在运行的该docker image中**

#### 3.1、 下载 anaconda

```shell
wget  https://repo.anaconda.com/archive/Anaconda3-5.2.0-Linux-x86_64.sh
```

#### 3.2、 安装 anaconda

```shell
Anaconda3-5.2.0-Linux-x86_64.sh: 350: Anaconda3-5.2.0-Linux-x86_64.sh: bunzip2: not found
tar: This does not look like a tar archive
tar: Exiting with failure status due to previous errors
root@890b8b636b88:~# yum install -y bzip2
-su: yum: command not found
root@890b8b636b88:~# apt-get install -y bzip2
```

#### 3.3、 安装依赖环境以及treeomics软件

按照  https://github.com/johannesreiter/treeomics 要求创建环境，安装相应得依赖环境

```shell
conda create  -n treeomics  python=3.4 NumPy SciPy networkx seaborn pandas matplotlib=1.5
apt-get update 
apt-get install git
git clone https://github.com/johannesreiter/treeomics.git
apt-get install wkhtmltopdf
pip install pdfkit
conda install perl=5.26.2
conda update -n base conda
apt-get install circos
apt-get install cpanminus
cpanm SVG
pip install varcode
pip install pyensembl
```

```shell
git clone https://github.com/johannesreiter/treeomics
```

#### 3.4、 安装 cplex

```shell
./cplex_studio126.linux-x86-64.bin
2
cd /opt/ibm/ILOG/CPLEX_Studio1261/cplex/python/3.4/x86-64_linux
python setup.py install
```

### 4、 测试

```shell
cd /root/treeomics/src
python treeomics -h
```

出现帮助信息，表明依赖安装完毕……

### 5、 docker 封装

新开一个shell 窗口，docker ps 获得我们docker 容器的id  890b8b636b88

exit 退出正在运行的 docker 容器 后

```shell
$ docker commit -m "a docker image for treeomics" -a "biolxy<biolxy@aliyun.com>" 890b8b636b88 ubuntu_treeomics
sha256:0bd8695dd6d647b249d7469eac397595ddbe40e7391415805d59f05d7d375e3d
```

![mark](http://oiz501hli.bkt.clouddn.com/blog/180919/JDa0kG25Jg.png?imageslim)

可以查看到我们创建的镜像了，OK

#### 5.1、 使用：

```shell
docker run -it -v `pwd`:`pwd` -w `pwd`  ubuntu_treeomics
```

### 6、 **参考**

- https://blog.csdn.net/leo15561050003/article/details/71274718?locationNum=14&fps=1  
- https://www.cnblogs.com/mthoutai/p/6812871.html