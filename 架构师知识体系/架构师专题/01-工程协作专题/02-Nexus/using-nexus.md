---
title: 配置使用Nexus存储库
shortTitle: 
description: 
icon: 
cover: 
author: 流浪码客
isOriginal: true
sticky: false
star: false
date: 2025-04-06
category: 分类
tags:
  - 标签
---
# 配置使用Nexus存储库
## 配置私服账户（`setting.xml`）
在 Maven 的 `settings.xml` 中添加 Nexus 认证信息，部署（`mvn deploy`）时使用。
```xml
<!-- servers
| 这是一个认证配置列表，每个认证配置通过 server-id 唯一标识（用于 Maven 系统内部引用）。
| 每当 Maven 需要连接到远程服务器时，就可以使用这些认证信息。
-->
<servers>
	<!-- 🔐 使用用户名/密码认证 -->
	<server>
      <id>maven-public</id>
      <username>admin</username>
      <password>123456</password>
    </server>
	<server>
	  <id>maven-releases</id>
	  <username>admin</username>
	  <password>123456</password>
	</server>
	<server>
	  <id>maven-snapshots</id>
	  <username>admin</username>
	  <password>123456</password>
	</server>
	<!-- maven2 proxy 仓库地址：https://repo.maven.apache.org/maven2/ -->
	<server>
      <id>maven-plugins</id>
      <username>admin</username>
      <password>123456</password>
    </server>
	<!-- 🔐（可选）使用私钥认证 
	<server>
	  <id>siteServer</id>
	  <privateKey>/path/to/private/key</privateKey>
	  <passphrase>可选；如果不使用，请留空。</passphrase>
	</server> -->
</servers>
```
* ⚠️`<id>` 必须和 `pom.xml` 中的 `<distributionManagement>`配置一致！
### [密码加密](https://maven.apache.org/guides/mini/guide-encryption.html)
为避免明文密码泄露，建议加密后再写入 `settings.xml`。
**1.创建主密码**
从 Maven 3.2.1开始，可不再使用密码参数`<password>`，在终端内根据提示输入，如<u>123456</u>
```shell 
mvn --encrypt-master-password 123456
```
**2.存储主密码（用于解密）**
将此密码存储在 `${user.home}/.m2/settings-security.xml` 中
```xml
<settingsSecurity>
	<master>{wG/VcOnQQoQHoWsQK+dSA5gyc2LkbEeRQLxpb4P5OVA=}</master>
</settingsSecurity>
```
**3.加密服务器密码**
```shell
mvn --encrypt-password <password>
```
4.**替换为加密后的密码**
```xml
  <servers>
    <!-- 🔐 使用用户名/密码认证 -->
    <server>
      <id>maven-public</id>
      <username>admin</username>
      <password>{IRAFt09LeK0H94U71R2ISoNGGuVkeHp1IwnnDtIc514=}</password>
    </server>
    <server>
      <id>maven-releases</id>
      <username>admin</username>
      <password>{wF2QWyyUOjkH+bqk0KZ2JszX0T03l/77zi6ulBfMQtM=}</password>
    </server>
    <server>
      <id>maven-snapshots</id>
      <username>admin</username>
      <password>{N2WpSLAJrKIHdfA3IWwZUgHa710U99t1uBB7FCFITKE=}</password>
    </server>
    <!-- maven2 proxy 仓库地址：https://repo.maven.apache.org/maven2/ -->
    <server>
      <id>maven-plugins</id>
      <username>admin</username>
      <password>{9R/aDMXnAegHkuz+P6PrkkSmQFqYQiz5xwIQ5K11qbs=}</password>
    </server>
</servers>
```
## 配置构件发布位置（`pom.xml`）
在项目的 `pom.xml` 中添加构件发布位置：
```xml
<!-- Maven 构件发布位置 -->
<distributionManagement>
  <!-- RELEASES 版本发布 -->
  <repository>
	<id>maven-releases</id>
	<name>Releases Repository</name>
	<url>http://nexus.orb.local/repository/maven-releases/</url>
  </repository>
  <!-- SNAPSHOT 版本发布 -->
  <snapshotRepository>
	<id>maven-snapshots</id>
	<name>Snapshots Repository</name>
	<url>http://nexus.orb.local/repository/maven-snapshots/</url>
  </snapshotRepository>
</distributionManagement>
```
**🎯 说明：**
* 带 `-SNAPSHOT` 后缀的版本将上传到 `snapshotRepository`
* 其他版本上传到 `repository`（正式库）
## 发布构件到私服
以官方教程 [Maven in 5 Minutes](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html) 创建的 `my-app` 为例，配置好后执行：
```shell
# SNAPSHOT 发布
mvn clean deploy
```
成功后，Maven 会自动将构建产物上传到私服。打开浏览器访问：
📍 [https://nexus.orb.local/](https://nexus.orb.local/)
进入 `Browse` 页面查看：
* `maven-releases`：正式版本仓库
* `maven-snapshots`：快照版本仓库
![Nexus Brow](http://img.geekyspace.cn/pictures/2025/20250407220612760.png)
## 配置全局 Profile（`settings.xml`）
使所有项目默认使用 Nexus 私服下载依赖。
```xml
<!-- 这是一个镜像源的列表，用于从远程仓库下载工件（artifacts）。 -->
<mirrors>
  <!-- Nexus 公共代理组（如 maven-public） -->
  <mirror>
    <id>maven-public</id>
    <mirrorOf>*</mirrorOf>
    <url>https://nexus.orb.local/repository/maven-public/</url>
  </mirror>
</mirrors>

<profiles>
  <!-- ✅ Nexus 仓库配置 Profile -->
  <profile>
    <id>nexus</id>
    <!-- 设置该配置默认启用，无需手动指定 -P 参数 -->
    <activation>
      <activeByDefault>true</activeByDefault>
    </activation>
    <!-- 配置项目依赖的仓库列表 -->
    <repositories>
      <repository>
        <id>maven-public</id>
        <url>https://nexus.orb.local/repository/maven-public/</url>
        <releases><enabled>true</enabled></releases>
        <snapshots><enabled>true</enabled></snapshots>
      </repository>
    </repositories>
    <!-- 插件仓库配置：用于下载如 compiler、surefire 等插件 -->
    <pluginRepositories>
      <pluginRepository>
        <id>maven-plugins</id>
        <url>http://nexus.orb.local/repository/maven-plugins/</url>
        <releases><enabled>true</enabled></releases>
        <snapshots><enabled>true</enabled></snapshots>
      </pluginRepository>
    </pluginRepositories>
  </profile>
</profiles>

<!-- 激活名为 nexus 的 profile（即便没设置 activeByDefault） -->
<activeProfiles>
  <activeProfile>nexus</activeProfile>
</activeProfiles>
```
  [🐟代码小抄-Maven配置Nexus私服](https://codecopy.cn/post/ztsg81)
  