---
title: 安装Jenkins
shortTitle: 
description: 
icon: 
cover: 
author: 流浪码客
isOriginal: true
sticky: false
star: false
date: 2025-06-01
category: 工程协作
tags:
  - Jenkins
---
## 安装Jenkins
官方文档：[安装Jenkins](https://www.jenkins.io/zh/doc/book/installing/)
参考老齐视频：[安装并初始化Jenkins](https://www.bilibili.com/video/BV1ag411h75v)
### War文件
1. [下载 Jenkins](http://mirrors.jenkins.io/war-stable/latest/jenkins.war).
2. 打开终端进入到下载目录.
3. 运行命令 `java -jar jenkins.war --httpPort=9090`.
4. 打开浏览器进入链接 `http://localhost:9090`.
5. 按照说明完成安装.
### Docker（推荐）
参考Github [官方 Jenkins Docker 镜像](https://github.com/jenkinsci/docker/blob/master/README.md#official-jenkins-docker-image)
**创建 Docker 卷**
```shell
docker volume create jenkins-data
```
**启动 Jenkins 容器**
```
docker run -u root -d \
  --name jenkins \
  --restart unless-stopped \
  -p 9090:8080 -p 50000:50000 \
  -v jenkins-data:/var/jenkins_home \
  -v /var/run/docker.sock:/var/run/docker.sock \
  jenkins/jenkins:lts
```
**使用宿主机 JDK 和 Maven**
```shell
docker run -u root -d \
  --name jenkins \
  --restart unless-stopped \
  -p 9090:8080 -p 50000:50000 \
  -v jenkins-data:/var/jenkins_home \
  -v $JAVA_HOME:/opt/java/11.0.26:ro \
  -v $MAVEN_HOME:/opt/maven/3.8.8:ro \
  -v /var/run/docker.sock:/var/run/docker.sock \
  jenkins/jenkins:lts
```
* ⚠️注意：Jenkins 镜像默认的 `JAVA_HOME` 是 `/opt/java/openjdk`
## 安装后设置向导
### 解锁 Jenkins
当您第一次访问新的Jenkins实例时，系统会要求您使用自动生成的密码对其进行解锁。
![Unlock Jenkins](http://img.geekyspace.cn/pictures/2025/20250602152448739.png)
查看密码
```shell
sudo docker exec -it jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```
### 自定义jenkins插件
选择左侧安装推荐的一组插件
![Customize Jenkins](http://img.geekyspace.cn/pictures/2025/20250602142500021.png)
等待插件安装...
![Installation Failures](http://img.geekyspace.cn/pictures/2025/20250602201042742.png)
安装失败后点击`Retry`重试直到成功；重试仍然失败，先点击`Continue`跳过。
### 创建第一个管理员用户
创建第一个`root`管理员用户，密码也设置为`root`方便学习。
![Create First Admin User](http://img.geekyspace.cn/pictures/2025/20250602201849899.png)
**实例配置**，指定Jenkins URL
![Instance Configuration](http://img.geekyspace.cn/pictures/2025/20250602202302224.png)
Jenkins 已准备就绪！
![Jenkins is ready!](http://img.geekyspace.cn/pictures/2025/20250602202622166.png)
### 安装中文插件（新版无效）
* [Locale plugin](https://plugins.jenkins.io/locale)
* [Localization: Chinese (Simplified)](https://plugins.jenkins.io/localization-zh-cn)
![Plugin Manager](http://img.geekyspace.cn/pictures/2025/20250602190701585.png)




