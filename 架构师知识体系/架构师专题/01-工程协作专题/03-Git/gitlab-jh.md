---
title: GitLab（极狐）
shortTitle: 
description: 
icon: 
cover: 
author: 流浪码客
isOriginal: true
sticky: false
star: false
date: 2025-06-07
category: 工程协作
tags:
  - Git
  - GitLab
  - 极狐
---
# GitLab（极狐）
> 更现代，功能更全的 Git 开源服务器。
## 部署Docker 
参考：[极狐GitLab Docker部署](https://gitlab.cn/docs/jh/install/docker.html)
最新版本：<u>18.0</u>
```shell
# 设置环境变量
export GITLAB_HOME=$HOME/docker-volumes/gitlab/
# 宿主机永久设置
echo 'export GITLAB_HOME=$HOME/docker-data-volumes/gitlab/' >> ~/.zshrc
source ~/.zshrc

# 创建挂载目录
mkdir -p $GITLAB_HOME/{config,logs,data}

# 运行极狐 GitLab，启用 amd64 模拟
sudo docker run --detach \
  --name gitlab-jh \
  --restart unless-stopped \
  --env TZ=Asia/Shanghai \
  --hostname gitlab.orb.local \
  --shm-size 2048m \
  --publish 80:80 --publish 8443:443 --publish 2222:22 \
  --volume $GITLAB_HOME/config:/etc/gitlab \
  --volume $GITLAB_HOME/logs:/var/log/gitlab \
  --volume $GITLAB_HOME/data:/var/opt/gitlab \
  registry.gitlab.cn/omnibus/gitlab-jh
```
**Docker卷方案**
⚠️ 避免在Mac M芯片上的权限问题，第一次可以启动，停止后无法启动。
```shell
# 创建 Docker 卷
docker volume create gitlab_config
docker volume create gitlab_logs
docker volume create gitlab_data

# 运行极狐 GitLab，启用 amd64 模拟，挂载 Docker 卷
sudo docker run --detach \
  --hostname gitlab.orb.local \
  --publish 80:80 --publish 8443:443 --publish 2222:22 \
  --name gitlab \
  --restart unless-stopped \
  --volume gitlab_config:/etc/gitlab \
  --volume gitlab_logs:/var/log/gitlab \
  --volume gitlab_data:/var/opt/gitlab \
  --shm-size 2048m \
  registry.gitlab.cn/omnibus/gitlab-jh
```
## 获取密码
首次登录使用用户名`root`，通过如下方式获取密码。
```shell
# Linux安装包方式
sudo cat /etc/gitlab/initial_root_password
# Docker安装方式
sudo docker exec -it gitlab grep 'Password:' /etc/gitlab/initial_root_password
```
* [华为云-修改初始密码](https://support.huaweicloud.com/bestpractice-ecs/zh-cn_topic_0142594569.html) ，设置一个简单密码：`root`/`jh@2025!`
* [极狐-设置系统默认语言为中文](https://jihulab.com/gitlab-cn/gitlab/-/issues/3685)
参考文档：
* [阿里云官方文档-部署GitLab代码托管平台](https://help.aliyun.com/zh/ecs/use-cases/deploy-and-use-gitlab)
* [Gitlab管理用户、组、权限(一)](https://www.cnblogs.com/zangxueyuan/p/9222014.html)
