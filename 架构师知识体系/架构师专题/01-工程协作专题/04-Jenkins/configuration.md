---
title: 配置Jenkins
shortTitle: 
description: 
icon: 
cover: 
author: 流浪码客
isOriginal: true
sticky: false
star: false
date: 2025-06-02
category: 工程协作
tags:
  - Jenkins
---
# 配置Jenkins
## 全局工具配置
路径：**Jenkins 管理 → 全局工具配置**
### JDK
- 勾选“自动安装”或手动指定本地 JDK 路径
- 命名建议使用 `jdk-<version>`，例如 `jdk-11`

![JDK](http://img.geekyspace.cn/pictures/2025/20250604222503156.png)
### Git
- 若系统未安装 Git，请勾选“自动安装”
- 建议确认 Jenkins 所在环境中 `git` 命令可用
![Git](http://img.geekyspace.cn/pictures/2025/20250604222548138.png)
### Maven
- 可以选择自动安装，或指定已安装的 Maven 路径
- 建议命名为 `maven-<version>`，如 `maven-3.8.8`
![Maven](http://img.geekyspace.cn/pictures/2025/20250604222657871.png)
## 插件安装

| 插件名称                                                            | 说明                         |
| --------------------------------------------------------------- | -------------------------- |
| [Git Parameter](https://plugins.jenkins.io/git-parameter)       | 支持构建参数中选择 Git 分支、标签等       |
| [Publish Over SSH](https://plugins.jenkins.io/publish-over-ssh) | 支持通过 SSH 发布构建产物或执行远程命令     |
| [Deploy to container](https://plugins.jenkins.io/deploy)        | 此插件允许您在构建成功后，将war文件部署到容器中。 |
插件安装路径：**Jenkins 管理 → 插件管理 → 可选插件**
![安装插件](http://img.geekyspace.cn/pictures/2025/20250602212810151.png)
### Publish Over SSH
#### Mac开启SSH远程登录
开启SSH服务后，Mac可以充当文件部署服务器，接收来自Jenkins的构建产物，实现自动化发布。当然，你也可以选择使用虚拟机、云服务器或其他远程主机作为部署目标。
**路径导航：系统偏好设置 → 共享，开启远程登录**
![勾选开启远程登录](http://img.geekyspace.cn/pictures/2025/20250607172836446.png)
**终端命令确认**
```shell
# 查看远程登录状态（是否开启）
sudo systemsetup -getremotelogin
# 开启 SSH 服务（远程登录）
sudo systemsetup -setremotelogin on
# 关闭 SSH 服务（远程登录）
sudo systemsetup -setremotelogin off
```
#### 配置SSH发布地址
**配置导航：Jenkins → 系统管理 → 系统设置 → Publish over SSH**
![配置SSH发布地址](http://img.geekyspace.cn/pictures/2025/20250607173706755.png)
**服务器验证**
1. 使用 SSH 密钥登录，可上传私钥。
2. 若使用密码登录，可展开 `Advanced` 勾选“Use password authentication, or use a different key”并填写密码。
点击`Test Configuration`按钮，提示 “Success”，说明连接和认证成功。