---
title: Jenkins 流水线入门与实践
shortTitle: 
description: 
icon: 
cover: 
author: 流浪码客
isOriginal: true
sticky: false
star: false
date: 2025-07-08
category: 工程协作
tags:
  - Jenkins
---
# Jenkins 流水线入门与实践
## 安装 Blue Ocean 插件
[Blue Ocean](https://plugins.jenkins.io/blueocean) 是 Jenkins 的可视化 UI 插件，将传统的控制台日志变成了**图形化阶段展示**。
## 入门示例：Hello World
1️⃣ 创建一个 **Pipeline 类型** 项目，命名为 `simple-api-pipeline`。
2️⃣ 进入项目配置页面 → 切换到「**Pipeline**」选项卡。
* **Definition**：选择 `Pipeline script`
* **Script**：右上角下拉选择 `Hello World` 模板
![Hello World](http://img.geekyspace.cn/pictures/2025/202507090244322.png)
保存配置后，返回项目主页，点击 [**Open Blue Ocean**](https://jenkins.orb.local/blue/organizations/jenkins/simple-api-pipeline)，再点击 **「运行」** 启动流水线。
![Blue Ocean-Hello阶段](http://img.geekyspace.cn/pictures/2025/202507091454449.png)
## 自定义 Jenkinsfile 脚本
将以下脚本保存至仓库根目录下的 `Jenkinsfile`，其中每个 `stage` 代表 CI/CD 的一个步骤，通过 [SCM](#Pipeline Script from SCM) 自动加载或本地测试：
```shell
pipeline {
    agent any

    environment {
        TAG = "${BRANCH_OR_TAG.replaceAll('/', '-')}"
    }

    stages {
        stage('Git拉取代码') {
            steps {
                echo '✅ Git拉取代码 SUCCESS'
            }
        }
        stage('Maven构建项目') {
            steps {
                echo '✅ Maven构建项目 SUCCESS'
            }
        }
        stage('SonarQube扫描') {
            steps {
                echo '✅ SonarQube扫描质量检查 SUCCESS'
            }
        }
        stage('Docker制作镜像') {
            steps {
                echo '✅ Docker制作镜像 SUCCESS'
            }
        }
        stage('Harbor推送镜像') {
            steps {
                echo '✅ Harbor推送镜像 SUCCESS'
            }
        }
        stage('SSH发布通知') {
            steps {
                echo '✅ SSH发布通知 SUCCESS'
            }
        }
    }
    post {
        success {
            echo '🎉 全部阶段 SUCCESS'
        }
        failure {
            echo '❌ 流水线执行失败，请查看日志'
        }
    }
}
```
## Pipeline Script from SCM
| 模式类型                     | 特点说明                                     |
| ------------------------ | ---------------------------------------- |
| Pipeline script          | 直接在 Jenkins 中手动编写脚本，适合快速测试               |
| Pipeline script from SCM | <u>从 Git 仓库中加载 Jenkinsfile</u>，更适合实际项目管理 |
进入项目配置页面 → 切换到「**Pipeline**」选项卡。
* **Definition**：选择 `Pipeline script from SCM`。
* **SCM**：选择 `Git`，指定仓库地址和凭证。
*  **Script Path**：默认 `Jenkinsfile`，表示流水线脚本路径，子目录示例：`ci/Jenkinsfile`。
![Pipeline Script from SCM](http://img.geekyspace.cn/pictures/2025/202507092103434.png)
保存配置后，返回项目主页，点击 [**Open Blue Ocean**](https://jenkins.orb.local/blue/organizations/jenkins/simple-api-pipeline)，再点击 **「运行」** 启动流水线。
![Blue Ocean-阶段显示](http://img.geekyspace.cn/pictures/2025/202507092112761.png)
## Pipeline 语法生成器
在编写 Jenkinsfile 时，你可以通过 Jenkins 提供的 [Pipeline Syntax](https://jenkins.orb.local/job/simple-api-pipeline/pipeline-syntax) 工具快速生成常用的流水线步骤语法。
[参考完整 Jenkinsfile 配置](https://github.com/joeljhou/simple-api-pipeline/blob/main/Jenkinsfile)
### Git拉取代码
**Pipeline Project → 配置 → 勾选「This project is parameterized」 → 添加参数（Add Parameter） → 选择 `Git Parameter`**
![启用 Git 参数化构建](http://img.geekyspace.cn/pictures/2025/202507101015324.png)
通过[Pipeline Syntax](https://jenkins.orb.local/job/simple-api-pipeline/pipeline-syntax)快速生成 `checkout` 语法，用于从 Git 仓库拉取代码。
![版本控制检出代码](http://img.geekyspace.cn/pictures/2025/202507101049686.png)生成后，复制代码，替换 `BRANCH_OR_TAG` 变量，以指定具体的分支或标签名称：
```groovy
stage('Git拉取代码') {
	steps {
		checkout scmGit(
			branches: [[name: "${BRANCH_OR_TAG}"]],
			extensions: [],
			userRemoteConfigs: [[
				credentialsId: 'ff5a1807-5c7d-487c-bfc9-e4817ef59ff2',
				url: 'http://gitlab.orb.local/joeljhou/simple-api-pipeline.git'
			]]
		)
	}
}
```
### Maven构建项目
通过[Pipeline Syntax](https://jenkins.orb.local/job/simple-api-pipeline/pipeline-syntax)快速生成 `sh` 语法，在 Jenkins 流水线中执行 Maven 构建命令。
![Maven构建项目](http://img.geekyspace.cn/pictures/2025/202507101716806.png)

```groovy
stage('Maven构建项目') {  
    steps {  
        withEnv(["PATH+MAVEN=${tool 'maven-3.8.8'}/bin"]) {  
            sh 'mvn clean package -DskipTests'  
        }  
    }  
}
```
### SonarQube扫描
```groovy
stage('SonarQube扫描') {
	steps {
	   withCredentials([string(credentialsId: '2fdcc713-2421-4296-9873-fb8df6ae4d20', variable: 'SONAR_TOKEN')]) {
			withEnv(["PATH+SONAR=${tool 'sonar-scanner-7.1'}/bin"]) {
				 sh """
					sonar-scanner \
						-Dsonar.host.url=http://sonarqube.orb.local \
						-Dsonar.projectKey=com.geekyspace:simple-api-pipeline \
						-Dsonar.projectName="${env.JOB_NAME}" \
						-Dsonar.projectVersion="${env.BRANCH_OR_TAG}" \
						-Dsonar.sources=./ \
						-Dsonar.java.binaries=target
				"""
			}
	   }
	}
}
```
### 构建并推送Harbor镜像
通过[Pipeline Syntax](https://jenkins.orb.local/job/simple-api-pipeline/pipeline-syntax)快速生成 `shPublisher: Send build artifacts over SSH` 语法。
![构建并推送 Harbor 镜像](http://img.geekyspace.cn/pictures/2025/202507102003621.png)

```groovy
stage('构建并推送Harbor镜像') {
	steps {
		sshPublisher(publishers: [sshPublisherDesc(configName: 'Mac主机',
			transfers: [sshTransfer(
				sourceFiles: 'target/*.jar docker/ Dockerfile',
				removePrefix: '',
				remoteDirectory: 'simple-api-pipeline',
				execCommand: '''
# 提取标签名（替换分支中的斜杠为破折号）
TAG=$(echo "$BRANCH_OR_TAG" | sed 's|/|-|g')
# 临时将 /usr/local/bin 加入 PATH，确保脚本中能直接调用 docker 命令
export PATH=/usr/local/bin:$PATH

# 1. 进入项目根目录（Dockerfile 所在位置）
cd simple-api-pipeline/
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
sed -i '' '/"currentContext":/i\\
"credsStore": "osxkeychain",
' ~/.docker/config.json
				'''
			)]
		)])
	}
}
```
### SSH发布通知
```shell
stage('SSH发布通知') {
	steps {
		sshPublisher(publishers: [sshPublisherDesc(configName: 'Mac主机',
			transfers: [sshTransfer(
				sourceFiles: 'deploy.sh',
				removePrefix: '',
				remoteDirectory: 'simple-api-pipeline',
				execCommand: '''
					cd /Users/joeljhou/CodeHub/joeljhou/jenkins-deploy/simple-api-pipeline
					sh deploy.sh proxy.harbor.orb.local repo simple-api-pipeline "$BRANCH_OR_TAG" "$PORT"
				'''
			)]
		)])
	}
}
```