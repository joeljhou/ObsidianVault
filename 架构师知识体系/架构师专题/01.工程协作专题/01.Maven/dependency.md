---
title: Maven 依赖机制
shortTitle: 
description: 深入理解 Maven 依赖机制：构建高效项目的基石
icon: 
cover: 
author: 流浪码客
isOriginal: true
sticky: false
star: false
date: 2025-03-27
category: 分类
tags:
  - 标签
---
# Maven 依赖机制
官方来源：[依赖机制介绍](https://maven.apache.org/guides/introduction/introduction-to-dependency-mechanism.html)
> 在 Java 项目开发的庞大生态系统中，Maven 作为一款强大的项目管理工具，其依赖管理机制发挥着至关重要的作用。无论是小型单模块项目，还是包含数百个模块的大型应用，Maven 都能有条不紊地处理项目依赖，确保构建过程的稳定与高效。
## 可传递性依赖发现
**作用：** Maven 自动解析并引入依赖项的依赖，减少手动管理的工作量。
**依赖冲突管理机制：**
* **依赖调节/仲裁（Dependency Mediation）**
	* 当同一个库的不同版本出现在依赖树中时，Maven 遵循 **“最近定义” 原则**。
* **依赖管理（Dependency Management）**
	* 在 `parent` 项目或 `dependencyManagement` 中统一管理依赖版本，确保版本一致性。
* **依赖范围（Scope）**
	* 通过 `compile`、`provided`、`runtime`、`test` 等不同范围控制依赖的传递性。
* **依赖排除（Exclusions）**
	* 可通过 `<exclusions>` 标签手动排除不需要的传递依赖，减少冲突。
* **可选依赖（Optional Dependencies）**
	* 使用 `<optional>true</optional>` 标记依赖，使其不会被传递给下游模块。
## 依赖范围/作用域（Scope）
**作用：** 依赖范围用于限制依赖项的传递性，并确定何时将依赖项包含在类路径中。
**6 个范围：**
* **compile**（编译）：
* **provided**（提供编译）：
* **runtime**（运行时）：
* **test**（测试）：
* **system**（系统）：
* **import**（导入）：
## 依赖管理：集中化与版本控制



## 导入依赖与 BOM POMs：优化依赖管理

**依赖查找**
- [Maven Central Repository](https://search.maven.org/)
- [MvnRepository](https://mvnrepository.com/)
- IntelliJ IDEA中使用 [Maven Search](https://plugins.jetbrains.com/plugin/17170-maven-search) 插件。


