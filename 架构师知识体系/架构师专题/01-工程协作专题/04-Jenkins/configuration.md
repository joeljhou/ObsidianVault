---
title: 配置Jenkins
shortTitle: 
description: 
icon: 
cover: 
author: 流浪码客
isOriginal: true
sticky: false
star: false
date: 2025-06-02
category: 工程协作
tags:
  - Jenkins
---
# 配置Jenkins
## 全局工具配置
路径：**Jenkins 管理 → 全局工具配置**
### JDK
- 勾选“自动安装”或手动指定本地 JDK 路径
- 命名建议使用 `jdk-<version>`，例如 `jdk-11`

![JDK](http://img.geekyspace.cn/pictures/2025/20250604222503156.png)
### Git
- 若系统未安装 Git，请勾选“自动安装”
- 建议确认 Jenkins 所在环境中 `git` 命令可用
![Git](http://img.geekyspace.cn/pictures/2025/20250604222548138.png)
### Maven
- 可以选择自动安装，或指定已安装的 Maven 路径
- 建议命名为 `maven-<version>`，如 `maven-3.8.8`
![Maven](http://img.geekyspace.cn/pictures/2025/20250604222657871.png)
