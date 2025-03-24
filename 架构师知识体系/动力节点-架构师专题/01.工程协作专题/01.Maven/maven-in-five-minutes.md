---
title: 5 分钟了解 Maven
shortTitle: 
description: 
icon: 
cover: 
author: 流浪码客
isOriginal: true
sticky: false
star: false
date: 2025-03-25
category: 分类
tags:
  - 标签
---
# 5 分钟了解 Maven
官方来源：：[maven-in-five-minutes](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html)
## 创建项目（命令行）
* 官方文档：[坐标的命名约定](https://maven.apache.org/guides/mini/guide-naming-conventions.html)
	```shell
	mvn archetype:generate \
	    -DgroupId=com.mycompany.app \
	    -DartifactId=my-app \
	    -DarchetypeArtifactId=maven-archetype-quickstart \
	    -DarchetypeVersion=1.5 \
	    -DinteractiveMode=false
	```
## Maven工程的目录结构
* 官方文档：[标准目录布局简介](https://maven.apache.org/guides/introduction/introduction-to-the-standard-directory-layout.html)
* IDEA 中的 Maven Archetype 模板
	* `org.apache.maven.archetypes:maven-archetype-archetype`→ 自定义 Archetype 模板
	* `org.apache.maven.archetypes:maven-archetype-j2ee-simple`→ <u>J2EE 应用</u>
	* `org.apache.maven.archetypes:maven-archetype-plugin`→ <u>Maven 插件开发</u>
	* `org.apache.maven.archetypes:maven-archetype-plugin-site`→ Maven 插件文档站点
	* `org.apache.maven.archetypes:maven-archetype-portlet`→ 门户网站 Portlet 开发
	* `org.apache.maven.archetypes:maven-archetype-profiles`→ <u>配置多环境</u>（Profiles）
	* `org.apache.maven.archetypes:maven-archetype-quickstart`→ <u>普通 Java 项目</u>
	* `org.apache.maven.archetypes:maven-archetype-site`→ 生成 Maven 站点
	* `org.apache.maven.archetypes:maven-archetype-site-simple` → 简单的 Maven 站点
	* `org.apache.maven.archetypes:maven-archetype-webapp`→ <u>Web 应用</u>
* [🐟代码小抄-Maven工程的目录结构](https://codecopy.cn/post/s6z4y5)
### 设计思想（约定>配置>编码）
Maven 作为 Java 构建工具，遵循 **“约定优于配置（Convention over Configuration）”** 的设计理念，并强调 **“先约定，再配置，最后编码”** 的原则。
优点：
✅ **减少重复配置，保持项目结构完整，统一团队标准**

## POM 文件
> POM 代表“**项目对象模型**”。它是 Maven 项目的 XML 表示（万物皆对象思想），保存在名为 `pom.xml` 的文件中。
* 官方文档：[POM参考](https://maven.apache.org/pom.html)
* [🐟代码小抄-默认的超级 POM](https://codecopy.cn/post/2jjqwx)
* [🐟代码小抄-pom.xml标签详解](https://codecopy.cn/post/op9ex9)


