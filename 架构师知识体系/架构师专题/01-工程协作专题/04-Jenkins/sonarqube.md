---
title: Jenkins 集成 SonarQube 代码审查
shortTitle: 
description: 
icon: 
cover: 
author: 流浪码客
isOriginal: true
sticky: false
star: false
date: 2025-06-18
category: 工程协作
tags:
  - SonarQube
---
# 集成 SonarQube 代码审查
主要官方来源：[官网](https://www.sonarsource.com/products/sonarcloud/) ｜ [文档](https://docs.sonarsource.com/sonarqube/) ｜ [Github](https://github.com/SonarSource/sonarqube)
> SonarQube 是一个持续的代码质量和安全管理平台。它通过静态分析帮助团队发现代码中的缺陷、漏洞和代码异味，从而提升软件质量和安全性。SonarQube 支持多种编程语言，可集成到现有的开发流程和持续集成工具中，实现自动化的代码审查。
## 安装部署环境
### PostgreSQL
**使用 Docker 安装 PostgreSQL**
[postgres官方镜像](https://hub.docker.com/_/postgres)
```shell
# 创建 Docker 卷
docker volume create pgdata

# 启动 PostgreSQL 15 容器
docker run -d \
  --name postgres \
  --restart unless-stopped \
  -e TZ=Asia/Shanghai \
  -e POSTGRES_USER=postgres \
  -e POSTGRES_PASSWORD=postgres \
  -p 5432:5432 \
  -v pgdata:/var/lib/postgresql/data \
  postgres:15
```
**创建 SonarQube 使用的数据库**
```sql
docker exec -it postgres psql -U postgres
CREATE DATABASE sonar;
\q
```
### SonarQube
**使用 Docker 安装 SonarQube**
[从 Docker 镜像安装 SonarQube Server 的开发版或企业版](https://docs.sonarsource.com/sonarqube-server/latest/setup-and-upgrade/install-the-server/installing-sonarqube-from-docker/)
```shell
# 创建 Docker 卷
docker volume create --name sonarqube_data
docker volume create --name sonarqube_logs
docker volume create --name sonarqube_extensions

# 明确版本（可选）
docker pull sonarqube:9.9.8-community

# 启动 SonarQube 容器（连接 PostgreSQL）
docker run -d \
  --name sonarqube \
  --restart unless-stopped \
  -e TZ=Asia/Shanghai \
  -e SONAR_JDBC_URL=jdbc:postgresql://postgres.orb.local:5432/sonar \
  -e SONAR_JDBC_USERNAME=postgres \
  -e SONAR_JDBC_PASSWORD=postgres \
  -p 9000:9000 \
  -v sonarqube_data:/opt/sonarqube/data \
  -v sonarqube_logs:/opt/sonarqube/logs \
  -v sonarqube_extensions:/opt/sonarqube/extensions \
  sonarqube:lts
```
打开后就是[登录页面](sonarqube.orb.local)，默认用户名密码为`admin/admin`，初次登录需要重置密码。
**镜像版本选择**
* `sonarqube:lts` 👉 `sonarqube:9.9.8-community`（截至 2025 年 6 月）
#### 安装`中文语言包`插件
![chinese pack](http://img.geekyspace.cn/pictures/2025/202506230118857.png)
重启后生效。
## Sonar代码质量分析
### 生成令牌Token
**登录 [SonarQube](https://sonarqube.orb.local/) → 点击右上角头像 → 我的账号 → 安全**
![Generate Tokens](http://img.geekyspace.cn/pictures/2025/202506241212411.png)
**记录你的令牌信息**
```shell
名称：sonarqube-access
类型：用户令牌
令牌：squ_d9de66c6d9564d3c5829047fd7597a0bff492b6e
```
### 项目创建三种方式
在 SonarQube 中，可以通过以下三种方式创建并分析项目：
#### 命令行传参
**1.进入[SonarQube Projects](https://sonarqube.orb.local/projects/create)  → 点击[手工](https://sonarqube.orb.local/projects/create?mode=manual)创建项目**
![手动创建项目](http://img.geekyspace.cn/pictures/2025/202507010020326.png)
**2.选择「本地分析项目 」→ 创建项目令牌**
![](http://img.geekyspace.cn/pictures/2025/202507010026976.png)
**3.使用 Maven 执行 SonarQube 扫描**
```shell
mvn clean verify sonar:sonar \
  -Dsonar.projectKey=simple-api-pipeline \
  -Dsonar.host.url=http://sonarqube.orb.local \
  -Dsonar.login=squ_d9de66c6d9564d3c5829047fd7597a0bff492b6e
```
命令执行后，打开 [SonarQube Projects](https://sonarqube.orb.local/projects) 页面：
![sonarqube项目](http://img.geekyspace.cn/pictures/2025/202506251503280.png)
#### 项目`pom.xml`中配置
SonarQube 服务器地址和令牌
```xml
<project>
    ...
    <properties>  
        <java.version>17</java.version>
        <!-- ⚠️ 不推荐将 Token 写入此处，可能导致泄露 -->
        <sonar.projectKey>simple-api-pipeline</sonar.projectKey>  
        <sonar.host.url>http://sonarqube.orb.local</sonar.host.url>  
        <sonar.login>squ_d9de66c6d9564d3c5829047fd7597a0bff492b6e</sonar.login> 
    </properties>
</project>
```
随后执行如下命令，使用项目中配置
```shell
mvn clean verify sonar:sonar
```
#### 全局`settings.xml`配置
**用户级别的 Maven 设置文件：`~/.m2/settings.xml`**
```xml
<profiles>
  <!-- ✅ SonarQube 代码扫描配置 Profile -->
  <profile>
    <id>sonar</id>
    <properties>
      <sonar.host.url>http://sonarqube.orb.local</sonar.host.url>
      <sonar.login>squ_d9de66c6d9564d3c5829047fd7597a0bff492b6e</sonar.login>
    </properties>
  </profile>
</profiles>

<activeProfiles>
  <activeProfile>sonar</activeProfile>
</activeProfiles>
```
随后执行如下命令，使用`settings.xml` 中配置
```shell
mvn clean verify sonar:sonar
mvn clean verify sonar:sonar -s ~/.m2/settings.xml
```
**全局 Maven 设置文件：`$MAVEN_HOME/conf/settings.xml`**
```shell
mvn clean verify sonar:sonar -s $MAVEN_HOME/conf/settings.xml
```
## Jenkins集成SonarQube
### 安装`SonarQube Scanner`插件
**Jenkins 管理 → 插件管理 → 可选插件**
![SonarQube Scanner](http://img.geekyspace.cn/pictures/2025/202506261942256.png)
**桥梁作用：**
该插件是 Jenkins 与 SonarQube 之间的连接桥梁，使 Jenkins 能在构建过程中集成 SonarQube，实现自动化代码质量扫描与分析。
### 配置SonarQube服务器
**Jenkins 管理 → 系统设置 → 「SonarQube servers」**
```shell
服务名称：sonarqube
服务地址：http://sonarqube.orb.local
添加令牌凭证：squ_d9de66c6d9564d3c5829047fd7597a0bff492b6e
```
![配置 SonarQube 服务](http://img.geekyspace.cn/pictures/2025/202506291924183.png)

**添加SonarQube Token凭证**
![添加 SonarQube 令牌凭证](http://img.geekyspace.cn/pictures/2025/202506291922053.png)
### 配置`SonarQube Scanner`工具（可选）
**Jenkins 管理 → 全局工具配置 → 「SonarQube Scanner」**
* Name：`sonar-scanner-7.1`
![sonar-scanner安装](http://img.geekyspace.cn/pictures/2025/202506292012485.png)
**工具作用：**  
这是实际执行代码扫描的命令行工具（即 `sonar-scanner`），用于扫描项目代码并将结果提交到 SonarQube。
### Jenkins任务中添加扫描步骤
**Freestyle Project → 配置 →「构建步骤（`Build Steps`）」
→ 在 Maven 打包后添加步骤：Execute SonarQube Scanner，执行代码质量扫描**
![Execute SonarQube Scanner](http://img.geekyspace.cn/pictures/2025/202507010211660.png)
**Analysis properties 配置示例**
* 💡 提示：点击`?`按钮可查看更多配置项说明
```shell
# 项目唯一标识（必填），格式通常是 groupId:artifactId
sonar.projectKey=com.geekyspace:simple-api-freestyle
# 项目元数据（以前是必需的，自 SonarQube 6.1 起是可选的）
sonar.projectName=${JOB_NAME}
sonar.projectVersion=${BRANCH_OR_TAG}
# 源代码目录路径（必填）
# 使用 './' 表示从项目根目录开始扫描，避免遗漏非标准目录下的源码文件。
# 如果源码都集中在标准路径，也可以改为 src/main/java 以减少无关扫描。
sonar.sources=./
# Java 编译输出目录（必填）
# 一般为 Maven 默认的 target/classes，而非整个 target，避免扫描非字节码文件。
sonar.java.binaries=target
```
**执行构建并查看结果**
执行Jenkins 项目构建，构建成功后，在SonarQube 界面中查看扫描结果。