---
title: Maven 属性
shortTitle: 
description: 掌握 Maven 属性，提升项目配置的可维护性。
icon: 
cover: 
author: 流浪码客
isOriginal: true
sticky: false
star: false
date: 2025-03-31
category: 工程协作
tags:
  - Maven
---
# Maven 属性
参考：[Baeldung-Maven 预定义属性](https://www.baeldung.com/maven-predefined-properties) ｜ [官网-POM基础-属性](https://maven.apache.org/pom.html#Properties)

> Maven 属性可以看作是值的占位符，它们的值可以在 `pom.xml` 文件。的任何地方通过 `${X}` 的格式引用，其中 `X` 是属性名。这些属性的值还可以作为插件的默认配置值，帮助简化配置和提高灵活性。
## 自定义全局属性（版本号）
**自定义全局属性**
```xml
<!-- 在 properties 里定义版本号，方便统一管理 -->
<properties>
  <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
  <junit.version>4.13.2</junit.version>
  <spring.version>6.1.11</spring.version>
</properties>
```
**依赖中使用**
```xml
<dependencies>
  <dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>${junit.version}</version>
    <scope>test</scope>
  </dependency>
  
  <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>${spring.version}</version>
  </dependency>

  <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>${spring.version}</version>
  </dependency>
</dependencies>
```

## Maven 内置属性

### 项目属性
| 属性名                               | 描述                      |
| --------------------------------- | ----------------------- |
| `${project.groupId}`              | 当前项目的组标识符。              |
| `${project.artifactId}`           | 当前项目的工件标识符。             |
| `${project.version}`              | 当前项目的版本号。               |
| `${project.description}`          | 项目的描述信息。                |
| `${project.url}`                  | 项目的主页 URL 。             |
| `${project.packaging}`            | 项目的打包类型，例如 `jar`、`war`。 |
### 构建属性
| 属性名                                    | 描述                                                        |
| -------------------------------------- | --------------------------------------------------------- |
| `${project.build.directory}`           | 项目的构建输出目录，默认是 `target`。                                   |
| `${project.build.outputDirectory}`     | 编译后的类文件输出目录，默认是 `target/classes`。                         |
| `${project.build.testOutputDirectory}` | 编译后的测试类文件输出目录，默认是 `target/test-classes`。                  |
| `${project.build.sourceEncoding}`      | 项目源代码的字符编码。                                               |
| `${project.build.finalName}`           | 构建产物的最终名称，通常是 `${project.artifactId}-${project.version}`。 |
### Java 环境属性
|属性名|描述|
|---|---|
|`${java.home}`|当前使用的 JDK 安装目录。|
|`${java.version}`|当前使用的 JDK 版本。|
|`${java.vendor}`|Java 供应商名称。|
|`${java.vendor.url}`|Java 供应商的 URL 。|
|`${java.class.version}`|Java 类格式版本。|
|`${java.specification.version}`|Java 规范版本。|
|`${java.specification.vendor}`|Java 规范供应商。|
|`${java.specification.name}`|Java 规范名称。|
### 系统属性
| 属性名                 | 描述                                  |
| ------------------- | ----------------------------------- |
| `${os.name}`        | 操作系统名称。                             |
| `${os.arch}`        | 操作系统架构。                             |
| `${os.version}`     | 操作系统版本。                             |
| `${user.home}`      | 用户的主目录路径。                           |
| `${user.name}`      | 当前用户的用户名。                           |
| `${user.dir}`       | 用户的当前工作目录。                          |
| `${file.separator}` | 文件路径分隔符（Windows 为 `\`，Unix 为 `/`）。  |
| `${path.separator}` | 路径分隔符（Windows 为 `;`，Unix 为 `:`）。    |
| `${line.separator}` | 行分隔符（Windows 为 `\r\n`，Unix 为 `\n`）。 |
### Maven 特定属性
| 属性名                           | 描述                                               |
| ----------------------------- | ------------------------------------------------ |
| `${maven.version}`            | 当前正在运行的 Maven 版本。                                |
| `${settings.localRepository}` | 本地 Maven 仓库路径，默认是 `${user.home}/.m2/repository`。 |
| `${settings.interactiveMode}` | 是否允许 Maven 在构建过程中请求输入，默认 `true`                  |
| `${settings.offline}`         | 是否离线运行 Maven，`true` 时仅使用本地仓库中的依赖项。               |
