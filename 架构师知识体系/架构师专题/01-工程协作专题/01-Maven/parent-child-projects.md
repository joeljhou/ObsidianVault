---
title: Maven父子工程
shortTitle: 
description: Maven创建多模块（父子聚合）项目
icon: 
cover: 
author: 流浪码客
isOriginal: true
sticky: false
star: false
date: 2025-04-01
category: 工程协作
tags:
  - Maven
---
# Maven父子工程
 [🐟代码小抄-Maven父子工程的pom.xml](https://codecopy.cn/post/d3jwqo)
## 核心职责
Maven 的父子工程结构主要用于 **简化多模块项目的管理**，通过<u>继承</u>和<u>聚合</u>机制，提高代码复用性和一致性。
**父工程（Parent Project）**
父工程用于 **管理子模块**，提供 **统一的依赖、插件和仓库配置**，但自身不包含实际业务代码。
* 打包方式：必须为 `pom`（`<packaging>pom</packaging>`）
* 核心职责：
	* 统一版本管理（依赖版本/插件版本）
	* 声明公共依赖（`dependencyManagement`）
	* 配置公共插件（`pluginManagement`）
	* 定义仓库信息（`repositories`）
	* 声明子模块列表（`modules`）
**子模块（Submodules）**
子模块是 **具体的业务代码单元**。
* 核心职责：
	* 必须声明父工程（`<parent>` 元素）
	* 继承父工程配置（可选择性覆盖）
	* 实际业务代码的承载单元
## 继承（Inheritance）
> Maven 继承是一种 **父 POM 共享配置** 的机制，子模块（子 POM）可以继承父 POM 的配置，而无需重复定义。

**可继承元素**

| 元素类型      | **典型配置项**                                | **继承规则**                                    |
| --------- | ---------------------------------------- | ------------------------------------------- |
| **基本信息**  | `groupId`, `artifactId`, `version`       | `groupId` 和 `version` 可继承，`artifactId` 不能继承 |
| **依赖管理**  | `dependencies`, `dependencyManagement`   | `dependencyManagement` 需手动引用                |
| **构建配置**  | `build`, `reporting`, `pluginManagement` | `pluginManagement` 需手动引用，`plugins` 会自动应用    |
| **环境配置**  | `repositories`, `distributionManagement` | 完全继承                                        |
| **自定义属性** | `properties`                             | 支持覆盖式继承                                     |
| **资源管理**  | `resources`, `testResources`             | 资源路径继承，可扩展                                  |
| **构建插件**  | `plugins`, `pluginManagement`            | `plugins` 直接继承，`pluginManagement` 需子模块引用    |
| **构建配置**  | `profiles`                               | 可继承，可在子模块覆盖                                 |

**代码示例：继承依赖管理**
父 POM：
```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <version>2.7.0</version>
        </dependency>
    </dependencies>
</dependencyManagement>
```
子 POM（需显式声明依赖）：
```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
</dependencies>
```
## 聚合（Aggregation）
> Maven 聚合是一种 **批量管理** *多个模块* 的方式。使用 **聚合（Aggregator）POM**，可以一次性构建多个子模块。
* 聚合特性
	* 由 **父工程** 通过 `<modules>` 列表声明所有子模块
	* 在父工程目录执行 `mvn install` 可 **构建所有子模块**
	* 子模块无需显式声明 `groupId` 和 `version`，直接继承父 POM
**代码示例：聚合 POM**
```xml
<!-- 子模块列表（聚合功能） -->
<modules>
	<module>module-a</module>
	<module>module-b</module>
</modules>
```
在 `parent-project` 目录下执行：
```shell
mvn clean install
```
将会 **递归构建所有子模块**。

## 总结
* **继承** 解决 **配置共享** 问题（减少重复定义）
* **聚合** 解决 **批量构建** 问题（提高构建效率）
* **两者可以同时使用**，但 **聚合不要求模块必须继承同一个父 POM**
