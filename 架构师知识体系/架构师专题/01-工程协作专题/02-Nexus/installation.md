---
title: 安装Nexus
shortTitle: 
description: 
icon: 
cover: 
author: 流浪码客
isOriginal: true
sticky: false
star: false
date: 2025-04-04
category: 工程协作
tags:
  - Nexus
---
# 安装Nexus
## 二进制安装
* 参考官网教程：[安装 Nexus 存储库](https://help.sonatype.com/en/install-nexus-repository.html)
## Docker
Nexus Repo Manager 3 的 Docker 版本：[Github docker-nexus3](https://github.com/sonatype/docker-nexus3) ｜ [Docker Hub](https://hub.docker.com/r/sonatype/nexus3)
**📌 快速启动**
```shell
$ docker run -d -p 8081:8081 --name nexus sonatype/nexus3
```
**📌 数据持久化**
1. 使用 docker 卷（推荐）
	```shell
	$ docker volume create --name nexus-data
	$ docker run -d -p 8081:8081 --name nexus -v nexus-data:/nexus-data sonatype/nexus3
	```
2. 将主机目录挂载为卷
	```shell
	$ mkdir /some/dir/nexus-data && chown -R 200 /some/dir/nexus-data
	$ docker run -d -p 8081:8081 --name nexus -v /some/dir/nexus-data:/nexus-data sonatype/nexus3
	```
**📌 访问Nexus**
* http://localhost:8081
**📌 获取admin用户的登陆密码**
```shell
$ docker exec -it nexus cat /nexus-data/admin.password
$ 1edb36b7-fe85-496e-afb4-789c4b82bbbf
```
* 修改密码为`123456`（学习使用，设置一个简单密码）
