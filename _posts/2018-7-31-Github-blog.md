---
layout: post
title: "Github个人博客搭建记录"
categories: 生活札记
tags: markdown 博客 图床 七牛
keywords: 搭建记录
description: Github个人博客搭建记录
---

* content
{:toc}

##
1. 在github账号下创建 biolxy.github.io   
2. git clone 该项目到本地
3. git clone 沈梦圆的博客到本地，填补博客必要的样式文件,删除_post文件加下md文件，保留一个做模板
```
git clone https://github.com/shenmengyuan/shenmengyuan.github.io.git 
mv shenmengyuan.github.io/* biolxy.github.io
```
4. 修改 CNAME _config.yml favicon.ico index.html page/3collections.md page/4about.md 等文件，替换为自己的信息
5. push 到github，打开 biolxy.github.io 即可看到效果
6. 注册七牛云，建立空间，做图床
7. 绑定MPic软件，方便上传图片及插入图片链接



