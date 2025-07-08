---
title: Harbor 搭建私有镜像仓库
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
  - Harbor
---
# 🐳 Harbor 搭建私有镜像仓库
主要官方来源：[官网](https://goharbor.io) ｜ [Github](https://github.com/goharbor/harbor)
> Harbor 是一个企业级 Docker 镜像仓库，用于存储和分发镜像。本文将介绍如何搭建 Harbor 私有镜像仓库，并通过 Jenkins 实现自动化构建和推送镜像。
## 安装
**详细步骤**
1. **下载 Harbor 离线安装包**
	[harbor-offline-installer-v2.13.1.tgz](https://github.com/goharbor/harbor/releases/download/v2.13.1/harbor-offline-installer-v2.13.1.tgz)
2. **解压**
3. **修改配置文件**：`harbor.yml`
	* 设置域名或 IP，例如：`hostname: proxy.harbor.orb.local`
	* 注释或删除 HTTPS 相关配置（内网部署可跳过）
	* 配置数据持久化目录`data_volume`，确保目录有读写权限，例如：`/Users/joeljhou/Services/harbor/data`
	* 参考详细配置：[🐟代码小抄-Harbor配置](https://codecopy.cn/post/vxniw0)
4. **执行安装脚本** ：`./install.sh`
***
**查看 Harbor 容器启动成功**
![harbor容器成功启动](http://img.geekyspace.cn/pictures/2025/202507021333756.png)
⚠️常见问题：
* 启动失败权限不足，`harbor.yml`中确认 `data_volume` 路径正确且具有读写权限。
* 不需要 TLS 认证，`docker-compose.yml`中将 `root.crt` 相关配置注释。
* Docker 重启时自动启动容器，建议将 `restart` 策略改为 `unless-stopped`。
***
**[访问 Harbor Web 控制台](https://proxy.harbor.orb.local/)**
* 用户名密码：`admin/Harbor12345`
![harbor首页](http://img.geekyspace.cn/pictures/2025/202507051020303.png)
## 上传镜像
将本地镜像 `dev-simple-api-pipeline` 上传到 [Harbor 私有仓库](https://proxy.harbor.orb.local)：
**1️⃣  新建 harbor 项目**
* 项目名称：`repo` / `public`
* 访问级别：公开
**2️⃣ 给镜像打标签（Tag）**
* 镜像命名规范：`<Harbor地址>/<项目名>/<镜像名>:<版本>`
```shell
# 查看当前本地镜像
docker images | grep simple-api-pipeline

# 为镜像添加 Harbor 仓库标签
docker tag dev-simple-api-pipeline:latest proxy.harbor.orb.local/repo/simple-api-pipeline:latest
```
**3️⃣ 登录 Harbor**
```shell
# 方式1：交互式登录
$ docker login proxy.harbor.orb.local
Username: admin
Password: 
Login Succeeded

# 方式2：通过 `-u` 和 `-p` 直接传递密码（不推荐）
$ docker login proxy.harbor.orb.local -u admin -p Harbor12345
WARNING! Using --password via the CLI is insecure. Use --password-stdin.
Login Succeeded

# 方式3：通过 `--password-stdin` 传递密码（推荐）
$ echo "Harbor12345" | docker login proxy.harbor.orb.local -u admin --password-stdin
Login Succeeded
```
**4️⃣ 推送镜像**
```shell
docker push proxy.harbor.orb.local/repo/simple-api-pipeline:latest
```
**5️⃣ 在 Harbor Web 页面查看**
* 登录 Harbor，进入项目 `repo`，确认镜像 `simple-api-pipeline` 已上传成功。
![harbor-repo-simple-api-pipeline](http://img.geekyspace.cn/pictures/2025/202507061615230.png)
[📢 Github Discussions: OrbStack 中 insecure-registries 配置无效 #2032](https://github.com/orgs/orbstack/discussions/2032)
## Jenkins 集成 Harbor
### 目前已经实现的逻辑
目前已经实现的逻辑：在 Jenkins 中通过 SSH 发送构建产物到远程目标机，远程执行Docker部署命令。
**Freestyle Project -> 配置 →「构建后操作（`Post-build Actions`）」→ 添加`Send build artifacts over SSH`**
![构建镜像并发布](http://img.geekyspace.cn/pictures/2025/202506142326275.png)
***
🎯 改造目标
* [构建并推送镜像至Harbor仓库](#构建并推送镜像至Harbor仓库)
* [目标服务器拉取镜像并部署](#目标服务器拉取镜像并部署)
### 构建并推送镜像至Harbor仓库
**方案1（推荐）：** `Build Steps`：通常在 Jenkins 或 Harbor 主机执行，借助映射宿主机 Docker 进行构建和发布。
**方案2（<u>Mac学习使用</u>）：**`Post-build Actions`：⚠️ 由于 Mac 上 Docker 与 Linux 指令集不同，无法直接通过映射执行构建，故在宿主机 Mac 终端直接执行相关命令。
```shell
# 提取标签名（替换分支中的斜杠为破折号）
TAG=$(echo "$BRANCH_OR_TAG" | sed 's|/|-|g')
# 临时将 /usr/local/bin 加入 PATH，确保脚本中能直接调用 docker 命令
export PATH=/usr/local/bin:$PATH

# 1. 进入项目根目录（Dockerfile 所在位置）
cd /Users/joeljhou/CodeHub/joeljhou/jenkins-deploy/simple-api-pipeline/
# 2. 构建 Docker 镜像，使用环境变量 TAG 作为镜像标签
docker build -t simple-api-pipeline:$TAG .
# 3. 给本地镜像打标签，指向 Harbor 私有仓库的对应版本标签，准备推送
docker tag simple-api-pipeline:$TAG proxy.harbor.orb.local/repo/simple-api-pipeline:$TAG
# 4. 临时移除 macOS Docker 配置文件中的 credsStore 配置，避免登录时 Keychain 报错
sed -i '' '/"credsStore":/d' ~/.docker/config.json
# 5. 无交互登录 Harbor 私有仓库，使用密码或 token 认证
echo "Harbor12345" | docker login proxy.harbor.orb.local -u admin --password-stdin
# 6. 推送带版本标签的镜像到 Harbor 私有仓库
docker push proxy.harbor.orb.local/repo/simple-api-pipeline:$TAG
# 7. 恢复 macOS Docker 配置文件中的 credsStore 配置，重新启用 Keychain 功能
sed -i '' '/"currentContext":/i\
    "credsStore": "osxkeychain",
' ~/.docker/config.json
```
![推送镜像到Harbor私有仓库](http://img.geekyspace.cn/pictures/2025/202507071901804.png)
随后点击构建，推送完成后，可以在 Harbor 仓库查看推送的镜像。
![harbor-repo-simple-api-pipeline](http://img.geekyspace.cn/pictures/2025/202507061615230.png)
### 目标服务器拉取镜像并部署
**Freestyle Project -> 配置 → General（通用） → 添加参数：「String Parameter」**
* 参数名：`PORT`
* 描述：指定部署服务监听端口（如 `8082`）
![服务监听端口](http://img.geekyspace.cn/pictures/2025/202507081438615.png)

**Freestyle Project -> 配置 →「构建后操作（`Post-build Actions`）」→ 添加`Send build artifacts over SSH`**
```shell
cd /Users/joeljhou/CodeHub/joeljhou/jenkins-deploy/simple-api-pipeline
sh deploy.sh proxy.harbor.orb.local repo simple-api-pipeline "$BRANCH_OR_TAG" "$PORT"
```
将 [deploy.sh](https://github.com/joeljhou/simple-api-pipeline/blob/main/deploy.sh) 脚本传至目标服务器指定目录并执行，实现自动部署。
![目标服务器拉取最新镜像并部署](http://img.geekyspace.cn/pictures/2025/202507081407610.png)
### 带参数构建测试
使用 Jenkins 的 **Build with Parameters** 功能，手动指定分支或标签及部署端口，触发构建流程并完成自动化部署。
![Build with Parameters构建测试](http://img.geekyspace.cn/pictures/2025/202507081505427.png)
镜像上传并拉取成功，容器根据指定的端口号启动。
![容器启动](http://img.geekyspace.cn/pictures/2025/202507081510899.png)
