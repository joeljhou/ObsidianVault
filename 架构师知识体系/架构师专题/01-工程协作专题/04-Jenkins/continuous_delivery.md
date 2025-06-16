---
title: 持续交付/部署（CD）
shortTitle: 
description: 
icon: 
cover: 
author: 流浪码客
isOriginal: true
sticky: false
star: false
date: 2025-06-15
category: 工程协作
tags:
  - Jenkins
---
# 持续交付/部署（CD）
## 创建分支或标签
| Git 分支/标签 | Controller 输出示例  |
| --------- | ---------------- |
| `main`    | `success main`   |
| `dev`     | `success dev`    |
| `v1.0.0`  | `success v1.0.0` |
| `v2.0.0`  | `success v2.0.0` 
## 安装插件`Git Parameter`
插件功能：构建时动态加载 Git 仓库中的分支和标签，方便选择构建版本
**Jenkins 管理 → 插件管理 → 可选插件**
![安装插件](http://img.geekyspace.cn/pictures/2025/20250602212810151.png)
## 配置参数化构建（分支/标签）
### 启用参数化构建
**Freestyle Project → 配置 → 勾选「This project is parameterized」 → 添加参数（Add Parameter） → 选择 `Git Parameter`**
![启用 Git 参数化构建](http://img.geekyspace.cn/pictures/2025/202506162241054.png)

| 配置项                | 设置值                        | 说明                |
| ------------------ | -------------------------- | ----------------- |
| **Name**           | `BRANCH_OR_TAG`            | 构建变量名，可自定义        |
| **Parameter Type** | `Branch or Tag`            | 同时支持选择分支或标签       |
| **Default Value**  | `origin/main`（或如 `v1.0.0`） | 默认值，建议设置为主分支或常用标签 |
| **Branch Filter**  | `.*`                       | 匹配所有分支            |
| **Tag Filter**     | `.*`                       | 匹配所有标签            |
| **Sort Mode**      | `Descending`               | 新的分支或标签在前         |
| **Quick Filter**   | 勾选                         | 启用快速搜索            |
### 构建前切换分支/标签
在 **「构建步骤（Build Steps）」** 中添加 **`Execute shell`**，执行以下命令：
```shell
git checkout $BRANCH_OR_TAG
```
该命令确保每次构建时，<u>代码检出选择的分支或标签</u>。
![Execute shell](http://img.geekyspace.cn/pictures/2025/202506162326088.png)
## 参数化构建测试
保存后，返回`simple-api-pipeline`项目主页，点击 **Build with Parameters**。
接着，在下拉框中选择你要构建的分支或标签（比如 `origin/main` 或 `v1.0.0`），点击 **“Build”** 启动构建流程。
![](http://img.geekyspace.cn/pictures/2025/202506162334232.png)
✅ 构建成功关键日志示例：
```shell
# 切换到参数指定的分支（如：origin/dev）
+ git checkout origin/dev
Previous HEAD position was 44b7db7 main
HEAD is now at ce87f5c dev分支
```
打开浏览器，访问部署服务地址：
```shell
http://localhost:8080/
```
返回`success dev`，表示 `origin/dev` 分支部署成功🎉！