---
title: Nexus发布构件
shortTitle: 
description: 
icon: 
cover: 
author: 流浪码客
isOriginal: true
sticky: false
star: false
date: 2025-04-10
category: 工程协作
tags:
  - Nexus
---
# Nexus发布构件
Nexus 是一个流行的私有 Maven 仓库，广泛用于发布和管理构件（如 JAR 包、WAR 包等）。
## web上传（jar）
首先，打开[阿里云云效Maven仓库](https://developer.aliyun.com/mvn/search)，搜索并找到你想要上传的依赖包。在本例中，我们选择了 `jenkins-client` 版本 `1.0.0`。
![jenkins-client-1.0.0.jar](http://img.geekyspace.cn/pictures/2025/20250410002558348.png)
接下来，登录到 Nexus 仓库的[Web Upload界面](http://localhost:8081/#browse/upload)。在`maven-releases`仓库上传下载好的包，并把相关信息填写完整：
*  **Group ID**： `com.offbytwo.jenkins`
*  **Artifact ID**： `jenkins-client`
* **Version**： `1.0.0`
![upload jenkins-client.1.0.0.jar](http://img.geekyspace.cn/pictures/2025/20250410003136625.png)
然后点击上传按钮。上传完成后，你可以在[仓库列表](http://localhost:8081/#browse/browse:maven-releases)中找到该构件，确保其已成功发布。
![search-maven](http://img.geekyspace.cn/pictures/2025/20250410003928022.png)
## `mvn`发布
除了 Web 界面上传构件外，使用 Maven 命令行进行构件发布是更常见的做法。这种方法适合自动化部署和持续集成。
### `pom`配置（工程）
> 参考：[[using-nexus]] 🚀 发布构件到私服，发布pom对应的当前工程。
### 命令行（jar）
我们把`v0.3.8`的包下载到本地，然后执行如下命令进行上传：
```shell
mvn deploy:deploy-file \
    -DgroupId=com.offbytwo.jenkins \
    -DartifactId=jenkins-client \
    -Dversion=0.3.8 \
    -Dfile=/Users/joeljhou/Downloads/lib/jenkins-client-0.3.8.jar \
    -Durl=http://localhost:8081/repository/maven-releases/ \
    -DrepositoryId=maven-releases \
    -s /Users/joeljhou/.sdkman/candidates/maven/3.6.3/conf/settings.xml
```
看到 `BUILD SUCCESS` 则说明上传成功。 来到仓库当中，也可以看到这个版本的依赖包了：
![upload jenkins-client.0.3.8.jar](http://img.geekyspace.cn/pictures/2025/20250410231142297.png)
