---
title: Git 安装
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
# Git 安装
##  Windows 安装
**官网版本安装**
* 访问 [Git 官网下载页面](https://git-scm.com/downloads/win)，选择你操作系统的版本，通常是 `64-bit Git for Windows Setup`。
**Git for Windows**
* 也叫做`msysGit`，和 Git 是分别独立的项目；
* 更多信息请访问 https://gitforwindows.org/ 。
**GitHub Desktop**
* 通过安装GitHub Desktop程序，轻松体验 Git。
* 官网号称终极GUI Git客户端，用于简化并增强你的本地代码管理流程。
* 网址为 https://github.com/apps/desktop ，支持Mac版。
## Linux 安装
**Debian/Ubuntu**
```shell
sudo apt-get install git
```
**Fedora**
```shell
sudo yum install git
```
要了解更多选择，Git 官方网站上有在各种 Unix 风格的系统上安装步骤，网址为 http://gitscm.com/download/linux。
## Mac 安装
**Homebrew**
* Git官网MacOS推荐使用Homebrew安装
```shell
brew install git
```
**Xcode Command Line Tools**
* 如果你的系统没有安装过 **Xcode Command Line Tools**，在安装 **Homebrew** 过程中，会自动提示你安装 Xcode Command Line Tools。
* 这个工具包包含了许多开发者工具，**Git** 是其中的一部分。
* 要了解更多安装选项，请访问 [Git 官网 Mac 安装页面](https://git-scm.com/downloads/mac)。