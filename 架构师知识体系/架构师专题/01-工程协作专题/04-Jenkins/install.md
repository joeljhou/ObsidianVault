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
* 官方镜像 + 插件自动安装，自动下载并配置 JDK 和 Maven。
* 使用官方自带 JDK 17 的镜像，直接启动 `jenkins/jenkins:lts-jdk17`。
* Jenkins 镜像默认的 `JAVA_HOME` 是 `/opt/java/openjdk`。
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
* ⚠️注意：必须挂载的是 Linux 平台的 JDK 和 Maven
```shell
docker run -u root -d \
  --name jenkins \
  --restart unless-stopped \
  -p 9090:8080 -p 50000:50000 \
  -v jenkins-data:/var/jenkins_home \
  -v $JAVA_HOME:/opt/java/11.0.26:ro \
  -v $MAVEN_HOME:/opt/maven/3.8.8:ro \
  -v /var/run/docker.sock:/var/run/docker.sock \
  jenkins/jenkins:lts-jdk17
```
**自定义 Dockerfile 构建镜像**
基于官方 Jenkins 镜像，自己写 Dockerfile，预装好需要的 JDK、Maven、工具，甚至插件，镜像一体化，启动即用。
* 编写 `Dockerfile`
```shell
# 使用官方 Jenkins 镜像，内置 JDK 17
FROM jenkins/jenkins:lts-jdk17
# 切换为 root 安装工具
USER root
# 1️⃣ 安装系统依赖（curl、unzip、ca-certificates）
RUN echo "🔧 安装系统依赖..." && \
    apt-get update && \
    apt-get install -y --no-install-recommends curl unzip ca-certificates
# 2️⃣ 下载并解压 Maven
ARG MAVEN_VERSION=3.8.8
ENV MAVEN_HOME=/opt/maven/apache-maven-${MAVEN_VERSION} \
    PATH=/opt/maven/apache-maven-${MAVEN_VERSION}/bin:$PATH
RUN echo "⬇️ 下载并安装 Maven ${MAVEN_VERSION} ..." && \
    curl -fsSL https://downloads.apache.org/maven/maven-3/${MAVEN_VERSION}/binaries/apache-maven-${MAVEN_VERSION}-bin.zip -o /tmp/maven.zip && \
    unzip /tmp/maven.zip -d /opt/maven && \
    rm -f /tmp/maven.zip
# 3️⃣ 清理无用缓存，精简镜像体积
RUN echo "🧹 清理缓存..." && \
    apt-get remove --purge -y curl unzip && \
    apt-get autoremove -y && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*
# 切回 Jenkins 用户
USER jenkins
# 4️⃣ 验证 Maven 安装，输出中文提示
RUN echo "✅ Maven 安装完成，当前版本：" && mvn -v
```
* 构建镜像，格式是 `[命名空间/仓库名]:[标签]`
```shell
docker build -t geekyspace/jenkins:lts-jdk17-maven3.8.8 .
```
* 发布镜像
```shell
docker login
# 如果镜像不带命名空间，需要给本地镜像打标签（添加命名空间）
docker tag jenkins:lts-jdk17-maven3.8.8 geekyspace/jenkins:lts-jdk17-maven3.8.8
# 推送带命名空间的镜像到 Docker Hub
docker push geekyspace/jenkins:lts-jdk17-maven3.8.8
```
* 运行 Jenkins 容器
```shell
docker run -u root -d \
  --name jenkins \
  --restart unless-stopped \
  -p 9090:8080 -p 50000:50000 \
  -v jenkins-data:/var/jenkins_home \
  -v /var/run/docker.sock:/var/run/docker.sock \
  geekyspace/jenkins:lts-jdk17-maven3.8.8
```
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

