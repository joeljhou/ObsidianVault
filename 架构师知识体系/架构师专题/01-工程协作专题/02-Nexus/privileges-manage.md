---
title: Nexus权限管理
shortTitle: 
description: 
icon: 
cover: 
author: 流浪码客
isOriginal: true
sticky: false
star: false
date: 2025-04-10
category: 工程协作
tags:
  - Nexus
---
# Nexus权限管理
参考资料：[access-control](https://help.sonatype.com/en/access-control.html)
> Nexus是基于[权限（Privileges）](https://help.sonatype.com/en/privileges.html)做访问控制的，服务器的每一个资源都有相应的权限来控制，因此用户执行特定的操作时就必须拥有必要的权限。管理员必须以[角色（Roles）](https://help.sonatype.com/en/roles.html)的方式将权限赋予Nexus[用户（Users）](https://help.sonatype.com/en/users.html)。

**关闭匿名访问**
* 出于安全考虑，推荐在 Security/Anonymous Access 中取消匿名访问的勾选。
**Blob Stores（对象存储）**
* 通常使用默认的`default`，不需要额外新建。

