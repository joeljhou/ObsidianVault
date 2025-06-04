---
title: 安装Maven
shortTitle: 
description: 
icon: 
cover: 
author: 流浪码客
isOriginal: true
sticky: false
star: false
date: 2025-03-16
category: 工程协作
tags:
  - Maven
---
# 安装Maven
主要官方来源：[Maven安装教程](https://maven.apache.org/install.html) 
> 注意：Maven的运行依赖需要JDK环境。通过设置 `JAVA_HOME` 和 `PATH` 来确保JDK可用。
## 二进制安装
1. **下载Maven**[二进制分发包](https://maven.apache.org/download.cgi)
	* macOS/Linux：`apache-maven-3.x.x-bin.tar.gz`
	* Windows：`apache-maven-3.x.x-src.tar.gz`
2. **解压Maven**
	* macOS/Linux：`/usr/local/maven/`
	* Windows：`C:\Program Files\maven\` 
3. **配置环境变量**
	* 将`apache-maven-3.x.x`的`bin`目录添加到`PATH`环境变量中
	```shell
	export M2_HOME=/usr/local/maven
	export PATH=$M2_HOME/bin:$PATH
	```
	* 执行`source ~/.zshrc`立即生效（macOS默认`zsh`，Linux使用 `source ~/.bashrc`）。 
4. **验证安装**
	```shell
	mvn -version
	```
## Homebrew
* Homebrew 默认会安装最新版的软件
```shell
# 安装 Maven
brew install maven
# 验证安装
mvn -version
# 更新 Maven
brew upgrade maven
```
## SDKMAN
* 稳定版本：`3.6.3`，配合 `jdk 1.8`
* `3.8.8`，配合 `jdk 11/17`
* `3.9.6`，配合 `jdk 17/21`
```shell
# 安装指定版本maven
sdk install maven 3.6.3
# 设置默认版本
sdk default maven 3.6.3
# 查看当前版本
sdk current maven
```

