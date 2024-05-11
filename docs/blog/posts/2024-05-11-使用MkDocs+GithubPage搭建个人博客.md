---
title: "使用MkDocs+GithubPage 搭建个人博客"
categories:
  - 博客
tags:
  - MkDocs
  - GithubPage
authors: [biolxy]
date: 2024-05-11
---


# 使用MkDocs+GithubPage 搭建个人博客

> 使用 MkDocs 的原因是 语雀没有会员的话，即使是(设置过的)互联网访问的文库，新创建的文档他人也无法打开链接，想来还是重新回转到博客了

## 1. 参考

- ref1: https://wcowin.work/blog/Mkdocs/mkdocs1.html

- ref2: https://www.cnblogs.com/chinjinyu/p/17610438.html
- 配置文件
  - https://zhuanlan.zhihu.com/p/688322635

## 2. 具体操作

具体操作参考 ref1中的步骤，有几个坑需要注意：
1. 我之前使用过GithuaPage，我没有删除旧项目，而是保留了`.git` 文件夹，清除了其文件，此时，我的主要分支是 `master`, 因此 `ci.yml` 中需要对于的修改：

   ```yaml
   name: ci 
   on: # 在什么时候触发工作流
     push: # 在从本地master分支被push到GitHub仓库时
       branches:
         - master
         - main
     pull_request: # 在master分支合并别人提的pr时
       branches:
         - master
         - main
   jobs: # 工作流的具体内容
     deploy:
       runs-on: ubuntu-latest # 创建一个新的云端虚拟机 使用最新Ubuntu系统
       steps:
         - uses: actions/checkout@v4 # 先checkout到main分支
         - uses: actions/setup-python@v4 # 再安装Python3和相关环境
           with:
             python-version: 3.x
         - run: pip install mkdocs-material # 使用pip包管理工具安装mkdocs-material
         - run: mkdocs gh-deploy --force # 使用mkdocs-material部署gh-pages分支
   ```

   

2. 使用 `blog` 插件，配置 `authors`  信息时，自23-08之后，`.authors.yml` 文件路径修改了，默认路径是在 `docs` 文件夹下：

   ```yaml
   authors:
     biolxy:
       name: biolxy    # Author name
       description: 生物信息工程师 # Author description
       avatar: https://gravatar.com/avatar/d63d3425433de59d2c3e901977a581c7?size=256&cache=1715411947733
   ```

   

   

