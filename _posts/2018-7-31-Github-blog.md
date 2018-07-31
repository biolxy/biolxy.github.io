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

# 搭建记录
1. 在github账号下创建 `biolxy.github.io`  
2. git clone 该项目到本地
3. git clone 沈梦圆的博客到本地，填补博客必要的样式文件,删除`_post`文件夹下文件，保留一个做模板，命名最好为全英文字符  
```shell
git clone https://github.com/shenmengyuan/shenmengyuan.github.io.git 
mv shenmengyuan.github.io/* biolxy.github.io
```
4. 修改如下等文件替换为自己的信息  
`CNAME _config.yml` `favicon.ico` `index.html` `page/3collections.md` `age/4about.md`  
5. 该主题更详细的信息需要去该主题作者的github查看[地址](https://github.com/Gaohaoyang/gaohaoyang.github.io/blob/master/README-zh-cn.md)
6. push到github，打开 `https://biolxy.github.io/` 即可看到效果
7. 注册七牛云，建立空间，做图床
8. 绑定MPic软件，方便上传图片及插入图片链接



