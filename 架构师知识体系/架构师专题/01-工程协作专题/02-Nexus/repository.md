---
title: 仓库管理
shortTitle: 
description: 
icon: 
cover: 
author: 流浪码客
isOriginal: true
sticky: false
star: false
date: 2025-04-05
category: 工程协作
tags:
  - Nexus
---
# 仓库管理
官方文档：[repository-management](https://help.sonatype.com/en/repository-management.html)
> **仓库（Repository）** 是用于存储和管理组件（如`jar`包`npm`包、`Docker镜像`等）的容器。
## 仓库信息
在 Nexus 中，通过 `Administration → Repository → Repositories` 访问仓库信息。
![Repositories界面](http://img.geekyspace.cn/pictures/2025/20250406001004499.png)
主要包括以下内容：
* **Name**：仓库名称（如 `maven-releases`）
* **Size**：仓库占用的存储空间
* [Formats](https://help.sonatype.com/en/formats.html)：仓库支持的构件格式（如 `Maven`、`npm`、`Docker`）
* **Blob Store**：二进制文件存储位置
* **Status**：仓库状态（如 `Online`、`Offline`）
* **URL**：仓库访问地址
* **Health Check**：仓库健康状态
* **Firewall Report**：防火墙报告
## 仓库类型
官方文档：[repository-types](https://help.sonatype.com/en/repository-types.html)
Sonatype Nexus 仓库支持三种主要的仓库类型：**代理仓库 (Proxy)**、**托管仓库 (Hosted)** 和 **组仓库 (Group)**。每种仓库类型都有其特定的使用场景和功能。
### 代理仓库 (Proxy)
- **功能**：代理仓库连接远程仓库，首次请求时从远程拉取并缓存，避免重复下载，提升效率。
* **默认代理仓库：**
	* [maven-central](https://search.maven.org/)：中央 Maven 仓库。
	* [nuget.org-proxy](https://www.nuget.org/)：`.NET` 默认仓库。
### 托管仓库 (Hosted)
* **功能**：托管仓库用于存储和发布组件，作为本地存储的权威位置。
* * **默认托管仓库：**
	* **maven-releases**：发布正式版本的 Maven 仓库。
	* **maven-snapshots**：发布开发版本（快照）的 Maven 仓库。
	* **nuget-hosted**：用于发布内部版本的 NuGet 仓库。
### 组/聚合仓库 (Group)
- **功能**：将多个仓库（包括代理和托管仓库）组合成一个仓库，简化访问。
- **默认组仓库：**
	- **maven-public**：结合中央仓库、`maven-releases` 和 `maven-snapshots`。
	- **nuget-group**：结合 `nuget-hosted` 和 `nuget.org-proxy`。
* **支持以下格式的组仓库**：
	* Docker
	* Go
	* Maven2
	* npm
	* NuGet
	* PyPI
	* R
	* Raw
	* RubyGems
	* Rust / Cargo
	* Yum（3.73.0 新增）
## 创建/删除仓库
参考官网：[创建和删除仓库](https://help.sonatype.com/en/creating-or-deleting-repositories-in-nexus-repository.html) ｜ [权限管理（Privileges）](https://help.sonatype.com/en/privileges.html)
目前使用默认仓库即可，不过多赘述，感兴趣可自行阅读官网内容。
