---
title: SSH管理
shortTitle: 
description: 
icon: 
cover: 
author: 流浪码客
isOriginal: true
sticky: false
star: false
date: 2025-06-07
category: 工程协作
tags:
  - Git
---
# SSH管理
##  生成 SSH 公钥
> 关于在多种操作系统中生成 SSH 密钥的更深入教程，请参阅 GitHub 的 SSH 密钥指南 [https://help.github.com/articles/generating-ssh-keys](https://help.github.com/articles/generating-ssh-keys)。
```shell
ssh-keygen -t ed25519 -C "your_email@example.com"
```
参考我的CSDN博客：
* [简单配置实现多个GitHub和Gitee账号的SSH管理](https://blog.csdn.net/qq_40174960/article/details/131298464)