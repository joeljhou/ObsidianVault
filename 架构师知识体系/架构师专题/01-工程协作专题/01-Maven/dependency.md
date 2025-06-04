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
category: 工程协作
tags:
  - Maven
---
# Maven 依赖机制
官方来源：[依赖机制介绍](https://maven.apache.org/guides/introduction/introduction-to-dependency-mechanism.html)
> 在 Java 项目开发的庞大生态系统中，Maven 作为一款强大的项目管理工具，其依赖管理机制发挥着至关重要的作用。无论是小型单模块项目，还是包含数百个模块的大型应用，Maven 都能有条不紊地处理项目依赖，确保构建过程的稳定与高效。
## 依赖声明与搜索
Maven 依赖的核心信息由 **GAV（GroupId、ArtifactId、Version）** 组成，所有依赖都存储在 **Maven 仓库** 中。
**📌 依赖声明**
在 `pom.xml` 文件中，我们通过 `<dependency>` 元素声明所需依赖：
```xml
<dependency>
	<groupId>组织ID</groupId>
	<artifactId>项目ID</artifactId>
	<version>版本号</version>
	<scope>依赖范围</scope>          <!-- 可选 -->
	<optional>true/false</optional> <!-- 可选，是否为可选依赖 -->
	<type>jar/pom</type>            <!-- 可选，默认是 jar -->
</dependency>
```
**🔍  依赖搜索**：
1. [Maven Central Repository](https://search.maven.org/)（官方仓库）
2. [MvnRepository](https://mvnrepository.com/)（第三方仓库）
3. IDEA 插件
	* [maven-search](https://plugins.jetbrains.com/plugin/17170-maven-search) ，快速查找`maven`，`gradle`依赖
	* [maven-helper](https://plugins.jetbrains.com/plugin/7158-maven-helper)， 右键 `pom.xml` → `Show Dependencies` 可查看依赖关系图
## 依赖传递与冲突管理
Maven 自动解析依赖项的依赖（即 **传递依赖**），但这可能导致版本冲突。
**📌 依赖传递性说明**
* A 依赖 B，B 依赖 C → A **间接** 依赖 C
* Maven 会自动下载 B 和 C，无需 A 手动声明 C
**📌 依赖冲突管理机制：**
* **依赖调节/仲裁（Dependency Mediation）**
	* 当同一个库的不同版本出现在依赖树中时，Maven遵循最近依赖（**<u>深度优先</u>**）原则。
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
**6 种范围：**
1. **compile**（编译）： 
	* 默认范围，适用于所有阶段（编译、测试、运行），并<u>可传递</u>。
	* 适用场景：大多数依赖，如 [spring-jdbc](https://mvnrepository.com/artifact/org.springframework/spring-jdbc)、[log4j](https://mvnrepository.com/artifact/log4j/log4j/1.2.17)。
2. **provided**（提供编译）：
	* 仅编译、测试可用，运行时不可用，不传递。
	* 适用场景：由外部容器（Tomcat、JDK）提供的依赖，如 [javax.servlet-api](https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api/4.0.1)，[lombok](https://mvnrepository.com/artifact/org.projectlombok/lombok/1.18.36)。
3. **runtime**（运行时）：⏳【运行时加载】
	* 测试、运行时可用，编译时不可用，可传递。
	* 适用场景：运行时需要但编译时不需要的依赖，如 `mysql-connector-java`（JDBC 驱动）。
4. **test**（测试）：
	* 仅测试阶段可用，不传递。
	* 适用场景：测试框架，如 [junit](https://mvnrepository.com/artifact/junit/junit/4.13.2)、[mockito-all](https://mvnrepository.com/artifact/org.mockito/mockito-all/1.10.19)。
5. **system**（系统）：
	* 编译、测试、运行可用。
	* 由本地文件系统提供，使用 `systemPath`标签指定具体位置，不传递。
	* 适用场景：本地 JAR 依赖（慎用，<u>不推荐</u>）。
6. **import**（导入）：
	* 仅用于 `<dependencyManagement>`，不添加具体依赖。
	* 适用场景：BOM（版本管理），如 **Spring Boot 统一管理依赖版本**。
## 依赖管理（dependencyManagement）
**作用**：在父 POM 统一定义依赖的版本、排除项等，子项目只需引用 `groupId` 和 `artifactId`，无需指定版本，避免冗余配置。
**示例**：
```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.apache.logging.log4j</groupId>
            <artifactId>log4j-core</artifactId>
            <version>2.14.1</version>
        </dependency>
        <dependency>
            <groupId>commons-logging</groupId>
            <artifactId>commons-logging</artifactId>
            <version>1.2</version>
        </dependency>
    </dependencies>
</dependencyManagement>
```
**优点**：
* 确保子项目使用的依赖版本一致，避免冲突
* 即使某个依赖的间接版本变化，也不会影响项目，保证构建稳定性
## 导入依赖（Import Scope）
**问题**：项目只能继承一个父 POM，多个独立项目需要共享依赖管理怎么办？
**解决方案**：使用 **导入依赖（Import Scope）** 或 **BOM（Bill of Materials）POM**。
```xml
<dependencies>
    <dependency>
        <groupId>com.example</groupId>
        <artifactId>projectA</artifactId>
        <version>1.0.0</version>
        <type>pom</type>
        <scope>import</scope>
    </dependency>
</dependencies>
```
🔹 这样，`projectA` 的 `dependencyManagement` 定义的所有依赖都会被当前 POM 引入。
## 物料清单 (BOM) 
BOM POM 是 **专门用来管理依赖版本** 的 POM，其他项目可通过 `import` 方式引入，统一版本管理，减少版本冲突。
**示例**（创建 BOM POM）：
```xml
<project>
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.example</groupId>
    <artifactId>example-bom</artifactId>
    <version>1.0.0</version>
    <packaging>pom</packaging>

    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-core</artifactId>
                <version>5.3.10</version>
            </dependency>
            <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-web</artifactId>
                <version>5.3.10</version>
            </dependency>
        </dependencies>
    </dependencyManagement>
</project>
```
**引入 BOM**：
```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>com.example</groupId>
            <artifactId>example-bom</artifactId>
            <version>1.0.0</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```
🔹 **优点**：
* 适用于多个项目共享一组稳定的依赖版本
* 只需导入 BOM，即可避免手动管理版本
* 提高依赖管理的一致性，减少版本冲突
## 系统依赖项（已弃用）
在 Maven 中，`system` 依赖范围（Scope）表示该依赖不会从 Maven 仓库中下载，而是从本地文件系统指定路径加载 JAR 文件。例如：
```xml
  <dependencies>
    <dependency>
      <groupId>javax.security</groupId>
      <artifactId>jaas</artifactId>
      <version>1.0.01</version>
      <scope>system</scope>
      <systemPath>${java.home}/lib/rt.jar</systemPath>
    </dependency>
    <dependency>
      <groupId>sun.jdk</groupId>
      <artifactId>tools</artifactId>
      <version>1.5.0</version>
      <scope>system</scope>
      <systemPath>${java.home}/../lib/tools.jar</systemPath>
    </dependency>
  </dependencies>
```
**替代方案**
本地安装 JAR 并上传至私有 Maven 仓库