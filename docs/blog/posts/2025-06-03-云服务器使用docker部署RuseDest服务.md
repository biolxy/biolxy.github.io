---
title: "云服务器使用docker部署RuseDest服务"
categories:
  - RuseDest
tags:
  - Linux
authors: [biolxy]
date: 2025-06-03
---


- [1.参照官网步骤](#1参照官网步骤)
  - [步骤 1：安装 Docker](#步骤-1安装-docker)
  - [步骤 2：下载 compose.yml](#步骤-2下载-composeyml)
  - [步骤 3：启动 Compose](#步骤-3启动-compose)
- [2.登录阿里云控制台，配置防火墙规则，开通以下端口](#2登录阿里云控制台配置防火墙规则开通以下端口)
- [3.连接验证](#3连接验证)




# 1.参照官网步骤

- https://rustdesk.com/zh-cn/
- https://blog.csdn.net/networken/article/details/140536087

## 步骤 1：安装 Docker

    bash <(wget -qO- https://get.docker.com)

## 步骤 2：下载 compose.yml

    wget rustdesk.com/oss.yml -O compose.yml

或

    wget rustdesk.com/pro.yml -O compose.yml

## 步骤 3：启动 Compose

    docker compose up -d

运行成功会在当前路径下生成 `data`文件夹其中密钥文件为`xx.pub`

    cat ./data/id_ed25519.pub

准备就绪！


# 2.登录阿里云控制台，配置防火墙规则，开通以下端口

放通TCP端口 `21115、21116、21117`
放通UDP端口 `21116`

# 3.连接验证
服务端搭建好后，在要连接的两个客户端设备上都下载 RustDesk 客户端。

下载地址是：https://github.com/rustdesk/rustdesk/releases

首先找到 RustDesk 客户端的 设置 -> ID/中继服务器 ，然后输入如下三个信息

ID 服务器：rustdesk.example.com:21116，默认端口为21116时可以省略端口配置

Key：填写部署服务默认生成的 id_ed25519.pub 文件中的内容

![](https://i-blog.csdnimg.cn/direct/02e393322bf24ed9b16456845f83d291.png)
