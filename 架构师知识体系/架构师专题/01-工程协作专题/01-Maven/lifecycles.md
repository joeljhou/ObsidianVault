---
title: Maven构建的生命周期
shortTitle: 
description: 掌握Maven构建的生命周期，以及对应的常见命令。
icon: 
cover: 
author: 流浪码客
isOriginal: true
sticky: false
star: false
date: 2025-03-27
category: 工程协作
tags:
  - Maven
---
# Maven构建的生命周期
官方来源：[生命周期参考](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html#Lifecycle_Reference)
## 三套生命周期
由 `maven-core` 模块中的[components.xml](https://maven.apache.org/ref/current/maven-core/lifecycles.html)文件定义，分为**三大阶段**：
 ### 清理（clean）
* 作用：删除 `target/` 目录，确保构建环境干净。
* 核心命令：`mvn clean`
 ### 默认（default）
* 作用：完整的构建流程，包括编译、测试、打包、安装和部署。
* 关键阶段：
	* `compile`（编译）
	* `test`（测试）
	* `package`（打包）
	* `install`（安装到本地仓库）
	* `deploy`（部署到远程仓库）
* 核心命令：`mvn package`、`mvn install` 等
 ### 站点（site）
* 作用：生成项目文档和报告，如 Javadoc、测试覆盖率报告等。
* 核心命令：`mvn site` 
## 核心命令
| 命令            | 作用                   |
| ------------- | -------------------- |
| `mvn clean`   | 清理项目，删除 `target/` 目录 |
| `mvn compile` | 编译源代码                |
| `mvn test`    | 运行单元测试               |
| `mvn package` | 打包项目（生成 JAR/WAR）     |
| `mvn install` | 安装到本地仓库              |
| `mvn deploy`  | 部署到远程仓库              |
| `mvn site`    | 生成项目文档               |
注意：执行以上命令必须在命令行进入`pom.xml` 所在目录！
## 总结
* **按顺序执行**：每个阶段都会执行之前的所有阶段。例如，执行 `mvn package` 时，会先执行 `validate` → `compile` → `test` → `package`。
- **插件驱动**：Maven 核心只定义了生命周期，具体工作由插件（如 `maven-compiler-plugin`）完成。
- **依赖管理**：通过 `pom.xml` 文件统一管理依赖，提高项目可维护性。
