---
title: settings配置文件
shortTitle: 
description: 
icon: 
cover: 
author: 流浪码客
isOriginal: true
sticky: false
star: false
date: 2025-03-17
category: 分类
tags:
  - 标签
---
# 配置Maven
主要官方来源：[配置Apache Maven](https://maven.apache.org/configure.html) ｜  [Settings.xml 的文档](https://maven.apache.org/settings.html)
## 环境变量配置
### `MAVEN_OPTS`
* 用于配置 JVM 参数，影响 Maven 的运行环境和性能。
```shell
# 初始堆内存 `256m` 和最大堆内存 `1024m`
export MAVEN_OPTS="-Xms256m -Xmx1024m"
```
### `MAVEN_ARGS`
* 用于配置 Maven 本身的运行参数，影响构建过程的具体行为（如跳过测试、指定插件目标等）。
```shell
# 每次执行 Maven 命令时自动跳过测试
export MAVEN_ARGS="-DskipTests"
```
## 全局`settings.xml`（重点）
用于配置 Maven 的全局设置，包括仓库、代理、镜像等。
* **全局配置**：`<Maven安装目录>/conf/settings.xml`
* **用户级配置**：`~/.m2/settings.xml`
### 配置本地仓库
```xml
<settings>
	<!-- 配置本地仓库 -->
	<localRepository>${user.home}/.m2/repository</localRepository>
</settings>
```
### 配置远程仓库/镜像
远程仓库是在 Maven 构建过程中获取依赖的地方。
```xml
<settings>
  <mirrors>
		<!-- 配置远程仓库镜像 -->
		<mirror>
		  <id>aliyunmaven</id>
		  <mirrorOf>*</mirrorOf>
		  <name>阿里云公共仓库</name>
		  <url>https://maven.aliyun.com/repository/public</url>
		</mirror>
  </mirrors>
</settings>
```
**Maven 远程仓库解析优先级（从高到低）**
1. `settings.xml` 的 `<mirrors>`（最高优先级，覆盖所有 POM 配置）
2. `pom.xml` 的 `<repositories>`（优先于默认中央仓库）
3. [超级 POM](https://www.codecopy.cn/post/2jjqwx)（默认中央仓库 `https://repo.maven.apache.org/maven2/`）
### 仓库镜像源列表/工件搜索
**仓库镜镜像源**
*  [阿里云](https://developer.aliyun.com/mvn/guide)
* [华为云](https://www.huaweicloud.com/special/maven-jingxiang.html)
* [腾讯云](https://mirrors.cloud.tencent.com/help/maven.html)
* [网易](https://mirrors.163.com/.help/maven.html)
**Maven仓库搜索**
- [Maven Central Repository](https://search.maven.org/)
- [MvnRepository](https://mvnrepository.com/)
- IntelliJ IDEA中使用 [Maven Search](https://plugins.jetbrains.com/plugin/17170-maven-search) 插件。

## 项目级.mvn 目录
`.mvn`位于项目**根目录**中的文件，用于存储Maven相关的配置文件。
1.  `.mvn/jvm.config`：用于配置 JVM 参数。
2. `.mvn/maven.config`：用于配置 Maven 的构建选项和参数。
3. `.mvn/extensions.xml`：用于配置 Maven 扩展。
### jvm.config
用于配置 JVM 参数，针对每个项目的 JVM 环境进行个性化设置
* **作用**：可以设置 JVM 的内存大小、垃圾回收选项、系统属性等。
* **示例配置**：
	```shell
	-Xmx2048m
	-XX:MaxPermSize=512m
	-Dfile.encoding=UTF-8
	```
### maven.config
`.mvn/maven.config` 文件用于存储 Maven 命令行常用的配置选项。这对于定义一组通用的 Maven 参数（如<u>并行构建、更新依赖、失败后继续</u>等）非常有用。
- **作用**：将 Maven 命令行选项集中在一个文件中，简化命令调用。
- **示例配置**：
	```shell
	-T3
	-U
	--fail-at-end
	```
### extensions.xml
在 Maven 3.2.5 之前，扩展通常需要手动放入 Maven 的 `${MAVEN_HOME}/lib/ext` 目录，或者通过 `mvn -Dmaven.ext.class.path=extension.jar` 在命令行上提供 jar 的路径。这种做法不够方便，并且需要手动管理。
- **作用**：从 Maven 3.3.1 及以后的版本开始，可以通过 `.mvn/extensions.xml` 文件来声明项目使用的扩展。
- **配置示例**：
```shell
<extensions xmlns="http://maven.apache.org/EXTENSIONS/1.1.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/EXTENSIONS/1.1.0 https://maven.apache.org/xsd/core-extensions-1.0.0.xsd">
  <extension>
    <groupId>com.example</groupId>
    <artifactId>example-extension</artifactId>
    <version>1.0.0</version>
  </extension>
</extensions>
```
这样，您只需要像使用任何其他 Maven 工件一样定义扩展的坐标。并且这些扩展的所有传递依赖项会自动从 Maven 仓库下载，避免了创建阴影工件的麻烦。

**其他指南：**
* [5 分钟了解 Maven](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html)
