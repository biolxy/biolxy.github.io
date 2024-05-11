---
layout: post
title: "我的第一个Dockerfile"
categories: 
  - Docker 
  - conda
tags: 
  - linux 
  - python
  - conda
description: 
authors: [biolxy]
mathjax: false
date: 2019-03-08
---

{:toc}







### 软件

该docker包含几个我常用的软件：

- 基于 CentOS Linux release 7.3.1611 (Core)

- axel    下载数据用
- git 
- zsh-syntax-highlighting.git    #  git 插件 谁用谁知道
- vim
- gcc g++ make  # 额这个没有，装这个 镜像体积太大了
- zsh
- miniconda 64bit python=2.7  

### Dockerfile 内容

```
FROM centos:7.3.1611
MAINTAINER biolxy <biolxy@goodrain.com>

# Set timezone
RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

RUN yum update -y
RUN yum install curl vim wget \
    gettext gettext-devel autoconf automake libtool openssl \
    openssl-devel zsh -y
RUN yum install bzip2 -y

# yum clean
RUN rm -rf /var/lib/apt/lists/*

# mkdir app
RUN mkdir -p /app
# Set WORKDIR
WORKDIR /app

# install axel
RUN git clone https://github.com/axel-download-accelerator/axel.git
RUN cd axel && autoreconf -i && ./configure && make && make install

# install zsh
RUN chsh -s `which zsh` root
COPY install.sh /app/install.sh
RUN chmod +x /app/install.sh
COPY oh-my-zsh /app/oh-my-zsh
RUN /app/install.sh
# install zsh-syntax-highlighting
RUN git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
# add plugins
RUN echo "plugins=(plugins zsh-syntax-highlighting)" >> ~/.zshrc
RUN echo "source \$ZSH/oh-my-zsh.sh" >> ~/.zshrc
# install conda
COPY Miniconda2-latest-Linux-x86_64.sh /app/Miniconda2-latest-Linux-x86_64.sh
RUN /bin/bash /app/Miniconda2-latest-Linux-x86_64.sh -b -p /opt/conda && \
    rm /app/Miniconda2-latest-Linux-x86_64.sh && \
    echo "export PATH=/opt/conda/bin:$PATH" >> ~/.zshrc
RUN echo "export PATH=/opt/conda/bin:$PATH" >> ~/.bashrc
RUN source ~/.bashrc && conda clean --all -y
```

###  创建docker image
```
docker build -t devtools:v3.0 .
```





参考

- https://www.oschina.net/question/584116_2209819