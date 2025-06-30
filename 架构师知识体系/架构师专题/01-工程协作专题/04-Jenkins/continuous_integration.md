---
title: 持续集成（CI）
shortTitle: 
description: 
icon: 
cover: 
author: 流浪码客
isOriginal: true
sticky: false
star: false
date: 2025-06-13
category: 工程协作
tags:
  - Jenkins
---
# 持续集成（CI）

## 创建Java Web项目
使用 Spring Initializr 创建 `simple-api-pipeline` 项目：
* 勾选 `Spring Web` 模块。
- 项目地址：[GitHub - simple-api-pipeline](https://github.com/joeljhou/simple-api-pipeline)
![simple-api-pipeline](http://img.geekyspace.cn/pictures/2025/202506090207404.png)
示例控制器代码：
```java
package com.geekyspace.controller;  
  
import org.springframework.web.bind.annotation.GetMapping;  
import org.springframework.web.bind.annotation.RestController;  
  
@RestController  
public class ExampleController {  
    @GetMapping("/")  
    public String home() {return "success";}  
}
```
## 推送至 GitLab
```shell
git remote add gitlab http://gitlab.orb.local/joeljhou/simple-api-pipeline.git
git push -u gitlab main
```
![simple-api-pipeline](http://img.geekyspace.cn/pictures/2025/202506100147580.png)

## 创建Freestyle项目
创建一个 **「Freestyle project」** 类型的`simple-api-pipeline`项目入门。
![Freestyle project](http://img.geekyspace.cn/pictures/2025/202506110047174.png)
### Git 源码管理
**Freestyle Project -> 配置 →「源代码管理（`Source Code Management`）」→ Git**
* 填写 gitlab 仓库地址
* 选择已添加的 gitlab 凭证（或`Add`新增）
* 指定构建的分支名称 `main`
![源代码管理](http://img.geekyspace.cn/pictures/2025/202506110216288.png)
保存后，返回`simple-api-pipeline`项目主页，点击 **「立即构建`Build Now`」**。
![Build Now](http://img.geekyspace.cn/pictures/2025/202506122046016.png)
随后查看`#5`的构建日志
![build-log](http://img.geekyspace.cn/pictures/2025/202506122051326.png)
日志显示在`workspace /var/jenkins_home/workspace/simple-api-pipeline`下构建成功，进入`jenkins`内部查看是否成功拉取到源代码。
![workspace](http://img.geekyspace.cn/pictures/2025/202506122058945.png)
### Maven 构建产物
**Freestyle Project → 配置 →「构建步骤（`Build Steps`）」**
* 选择 **Invoke top-level Maven targets（调用顶层 Maven 目标）** 作为构建方式
* 在 **Goals** 输入框中填写构建命令：
```shell
clean package -DskipTests
```
![Build Steps](http://img.geekyspace.cn/pictures/2025/202506122117544.png)
保存后，返回`simple-api-pipeline`项目主页，点击 **「立即构建`Build Now`」**。
![mvn clean package -DskipTests](http://img.geekyspace.cn/pictures/2025/202506130000672.png)
日志中看到执行了`mvn clean package -DskipTests`，并在`/target`目录下生成了`simple-api-pipeline-0.0.1-SNAPSHOT.jar`
### SSH 发送构建产物
#### 开启Mac SSH服务（远程登录）
<u>使Jenkins能通过SSH访问Mac作为远程部署目标《》。</u>你也可以选择使用虚拟机、云服务器或其他远程主机作为目标服务器。
**系统偏好设置 → 共享 → 勾选「远程登录」**
![勾选开启远程登录](http://img.geekyspace.cn/pictures/2025/20250607172836446.png)
📟 **终端命令确认状态**
```shell
# 查看远程登录状态（是否开启）
sudo systemsetup -getremotelogin
# 开启 SSH 服务（远程登录）
sudo systemsetup -setremotelogin on
# 关闭 SSH 服务（远程登录）
sudo systemsetup -setremotelogin off
```
#### 安装插件`Publish Over SSH`
**Jenkins 管理 → 插件管理 → 可选插件**
![安装插件](http://img.geekyspace.cn/pictures/2025/20250602212810151.png)
#### Jenkins配置SSH登录
**Jenkins 管理 → 系统设置 → 「Publish over SSH」**
![配置Jenkins SSH 服务器](http://img.geekyspace.cn/pictures/2025/202506140054112.png)
身份验证方式：
1. SSH 密钥登录（推荐）
2. 密码登录：展开 **Advanced**，勾选「Use password authentication」，填写密码即可
✅ 点击 `Test Configuration`，提示 “Success” 表示连接和认证成功。
#### 通过SSH发送构建工件
**Freestyle Project -> 配置 →「构建后操作（`Post-build Actions`）」→ 添加`Send build artifacts over SSH`**
- 选择 SSH 服务器，填写构建产物路径 `target/*.jar`，默认保留目录结构
- 填写 **Remove prefix（可选）** 去除本地路径前缀
- 填写 **Remote directory（可选）** 指定远程子目录
![Send build artifacts over SSH](http://img.geekyspace.cn/pictures/2025/202506140136675.png)
保存后，返回`simple-api-pipeline`项目主页，点击 **「立即构建`Build Now`」**。
```shell
# 构建日志输出
......
SSH: Connecting from host [8b569ba7b242]
SSH: Connecting with configuration [Mac主机] ...
SSH: Disconnecting configuration [Mac主机] ...
SSH: Transferred 1 file(s)
Finished: SUCCESS
```
📦 构建产物已通过 SSH 成功上传至远程目录：
![SSH发布构建到指定位置](http://img.geekyspace.cn/pictures/2025/202506140201654.png)
### 远程执行 Docker 部署
#### 编写Dockerfile
在项目根目录下创建 `Dockerfile`，用于构建 Spring Boot 容器镜像：
```dockerfile
# 使用 Eclipse Temurin JRE 17 作为基础镜像，体积较小
FROM eclipse-temurin:17-jre  
# 设置工作目录
WORKDIR /app
# 复制构建好的 JAR 文件到容器中  
COPY target/*.jar app.jar  
# 暴露 Spring Boot 默认端口
EXPOSE 8080
# 运行 Spring Boot 应用
ENTRYPOINT ["java", "-jar", "app.jar"]
```
#### 配置Docker Compose
在 `docker/dev/docker-compose.yml` 中添加服务配置：
```shell
version: '3.8'  
services:  
  simple-api-pipeline:  
    build:  
      context: ../..  
      dockerfile: Dockerfile  
    container_name: simple-api-pipeline  
    ports:  
      - "8080:8080"
```
> 完成以上步骤后，推送项目至 GitLab。
#### Docker容器部署启动
在 Jenkins 中完成[通过 SSH 发送构建产物](#%E9%80%9A%E8%BF%87SSH%E5%8F%91%E9%80%81%E6%9E%84%E5%BB%BA%E4%BA%A7%E7%89%A9)后，确保以下文件上传至远程服务器：
```shell
target/*.jar
docker/
Dockerfile
```
接着，在 **「远程执行命令 `Exec command`」** 中添加以下命令，完成容器重建和启动：
```shell
cd /Users/joeljhou/CodeHub/joeljhou/jenkins-deploy/simple-api-pipeline/docker/dev
/usr/local/bin/docker-compose down
/usr/local/bin/docker-compose up -d --build
/usr/local/bin/docker image prune -f
```
![构建镜像并发布](http://img.geekyspace.cn/pictures/2025/202506142326275.png)
保存后，返回`simple-api-pipeline`项目主页，点击 **「立即构建`Build Now`」**。
⚠️注意：**为什么会构建不成功？**
1. **docker 命令未使用全路径**
	* Jenkins 远程执行环境的 `PATH` 变量可能不完整，找不到 `docker` 或 `docker-compose` 命令导致失败。
	* ➡️ 解决：命令中使用完整路径，例如 `/usr/local/bin/docker-compose`。
2. **拉取镜像需要 Docker 登录**
	* 若未登录或凭据失效，拉取镜像时会失败，导致构建失败。macOS 下的 Keychain 在无交互环境中无法解锁，常见于自动化环境。
	* ➡️ 解决：提前在远程服务器登录 Docker 或预先拉取所需镜像，避免构建时再拉。
#### 验证部署成功
日志最后输出`Finished: SUCCESS`表示流水线执行完毕，随后验证容器是否启动。
![镜像部署成功](http://img.geekyspace.cn/pictures/2025/202506142329097.png)
打开浏览器，访问部署服务地址：
```shell
http://localhost:8080/
```
返回`success`表示部署成功 🎉！
