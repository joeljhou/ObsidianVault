---
title: Nexus 配置阿里云代理远程仓库
shortTitle: 
description: 
icon: 
cover: 
author: 流浪码客
isOriginal: true
sticky: false
star: false
date: 2025-04-09
category: 工程协作
tags:
  - Nexus
---
# Nexus 配置阿里云代理远程仓库
> 在国内使用 Nexus 作为 Maven 仓库代理时，为了提高构建速度并确保依赖稳定性，我们可以配置阿里云的 Maven 代理仓库。

下面是详细的配置步骤：
**1.创建代理`proxy`仓库**：
* 在 Nexus [管理界面](http://localhost:8081/#admin/repository)中，选择 **Repository** -> **Create repository**。
* 选择 **maven2 (proxy)** 类型。
* 填写仓库名称（例如：`nexus-aliyun`）。
**2.配置远程存储地址**：
* 在 **Remote Storage Location** 输入阿里云的 Maven 仓库地址：
	```shell
	https://maven.aliyun.com/repository/public
	```
* **国内仓库镜镜像源**
	*  [阿里云](https://developer.aliyun.com/mvn/guide)
	* [华为云](https://www.huaweicloud.com/special/maven-jingxiang.html)
	* [腾讯云](https://mirrors.cloud.tencent.com/help/maven.html)
	* [网易](https://mirrors.163.com/.help/maven.html)
![创建nexus-aliyun代理仓库](http://img.geekyspace.cn/pictures/2025/20250409192034108.png)**3. 点击底部的 Create repository**
**4.将 `nexus-aliyun` 添加到 `maven-public`**
* 在 Nexus 管理界面中，选择 **Repository -> maven-public**。
* 进入 **maven-public** 仓库配置页面。
* 选择之前创建的 `nexus-aliyun` 仓库，将其添加到仓库组中。
* 确保 `nexus-aliyun` 仓库的优先级高于中央仓库（如 `central`），以便优先使用阿里云代理仓库。
![添加仓库到group](http://img.geekyspace.cn/pictures/2025/20250409192319375.png)