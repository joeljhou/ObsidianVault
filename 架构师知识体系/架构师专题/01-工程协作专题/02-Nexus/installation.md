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
```shell
# 创建 Docker 卷
docker volume create --name nexus_data
# 明确版本（可选）
docker pull sonatype/nexus3:3.81.1
# 运行 Nexus 容器
docker run -d \
  --name nexus \
  --restart unless-stopped \
  -e TZ=Asia/Shanghai \
  -p 8081:8081 \
  -v nexus_data:/nexus-data \
  sonatype/nexus3
```
**📌 访问[Nexus](https://nexus.orb.local/)**
**📌 获取`admin`用户的登陆密码**
```shell
$ docker exec -it nexus cat /nexus-data/admin.password
$ 80fb9a61-9387-4d77-84e0-bfa8f9598615
```
* 修改密码为`123456`（学习使用，设置一个简单密码）
