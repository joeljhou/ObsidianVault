---
title: 概述及历史
shortTitle: 
description: 
icon: 
cover: 
author: 流浪码客
isOriginal: true
sticky: false
star: false
date: 2025-07-13
category: 工程协作
tags:
  - Linux
---
# 概述及历史
> **Linux 是一个自由和开源的类Unix 操作系统内核，由林纳斯·托瓦兹（Linus Torvalds）于1991年10月5日首次发布。** 它基于Unix 的设计思想，但独立开发，并在全球范围内广受欢迎和使用，尤其在服务器、嵌入式系统和高性能计算领域占据重要地位。
## Linux 概览
1. **开源自由：** Linux 及其组件遵循开源和自由软件的原则，允许用户自由使用、修改和分发。
2. **类Unix 系统：** 继承了Unix 的基本设计思想，支持多用户、多任务、多线程和多CPU。
3. **内核与发行版：**
	- **Linux** 严格来说是单指操作系统的内核。
	- 如今Linux常用来指基于Linux的完整操作系统，内核则改以[Linux内核](https://zh.wikipedia.org/wiki/Linux%E5%86%85%E6%A0%B8)称之。
	- 常见的Linux 发行版包括`Debian`、`Ubuntu`、`Fedora`、`Red Hat` 等。
4. **核心组件：** 
	-  **Linux 内核：** 提供设备驱动、进程管理、内存管理、文件系统、网络协议栈等底层功能。
	-  **系统库：** 包括 C 标准库（`libc`）、数学库（`libm`）、动态链接库（`libdl`）、线程库（`libpthread`）及第三方库，供应用程序调用系统资源。
	-  **Shell：** 命令行解释器，如 `bash`、`zsh`，用于执行命令和脚本，实现用户与系统的交互。
	-  **应用程序：** 用户空间的软件，如 Vim、Git、Nginx、MySQL 等，基于内核和系统库运行。
## 演进历程  
1. **早期灵感与先驱：**
	-  Linux 的设计思想深受 [Unix](https://zh.wikipedia.org/wiki/UNIX) 和 [MINIX](https://zh.wikipedia.org/wiki/MINIX) 操作系统的影响。
	-  Unix 最初起源于1964年的 Multics 项目，后由[肯·汤普逊](https://zh.wikipedia.org/wiki/%E8%82%AF%C2%B7%E6%B1%A4%E6%99%AE%E9%80%8A)和[丹尼斯·里奇](https://zh.wikipedia.org/wiki/%E4%B8%B9%E5%B0%BC%E6%96%AF%C2%B7%E9%87%8C%E5%A5%87)于1969年至1971年间用C语言开发。
	-  MINIX 是由[塔能鲍姆](https://zh.wikipedia.org/wiki/%E5%AE%89%E5%BE%B7%E9%B2%81%C2%B7%E6%96%AF%E5%9B%BE%E5%B0%94%E7%89%B9%C2%B7%E5%A1%94%E8%83%BD%E9%B2%8D%E5%A7%86)开发的用于教学的小型Unix 系统。
2. **Linus Torvalds 的贡献：**
	-  芬兰学生[林纳斯·托瓦兹](https://zh.wikipedia.org/wiki/%E6%9E%97%E7%BA%B3%E6%96%AF%C2%B7%E6%89%98%E7%93%A6%E5%85%B9) 在1991年受到MINIX 的启发，开始开发一个适合个人电脑的自由类Unix 系统，并于当年10月5日首次发布了Linux 内核。
3. **与GNU 项目的结合：**
	-  [GNU 通用公共许可证（GPL）](https://zh.wikipedia.org/wiki/GNU%E9%80%9A%E7%94%A8%E5%85%AC%E5%85%B1%E8%AE%B8%E5%8F%AF%E8%AF%81)
	-  自由软件基金会（FSF）的**Richard Stallman** 于1983年发起了GNU 项目，旨在构建一个完全自由的类Unix 操作系统。Linux 内核的出现与GNU 项目的大量组件相结合，形成了完整的GNU/Linux 系统，极大地推动了Linux 的发展。
4. **早期发展与成熟：**
	-  1994年，Linux 1.0 版本正式发布。随着社区的共同努力，Linux 内核不断发展，支持多处理器和图形界面，在全球范围内得到广泛应用。
## 历史时间线  

| 年份             | 事件                  | 简述                                              |
| -------------- | ------------------- | ----------------------------------------------- |
| **1969**       | Unix 雏形诞生           | 肯·汤普逊和丹尼斯·里奇开始在贝尔实验室开发 Unix，强调小程序组合与文本流         |
| **1971**       | 第一版 Unix 发布         | 用汇编语言编写，运行在 PDP-11 上                            |
| **1973**       | Unix 改用 C 语言重写      | 提高了可移植性，成为后续操作系统的基础                             |
| **1983**       | GNU 项目启动            | 理查德·斯托曼提出构建自由的 Unix 替代系统                        |
| **1987**       | MINIX 诞生            | 安德鲁·塔能鲍姆发布 MINIX 教学操作系统，开放源代码供教学使用              |
| **1991.08**    | Linus 发表公告          | 林纳斯·托瓦兹在 Usenet 上发帖，宣布正在开发一个自由内核项目              |
| **1991.10.05** | Linux 0.01 发布       | Linux 首个公开版本发布，基于 x86 架构                        |
| **1992**       | GPL 协议下发布           | Linux 内核改为采用 GNU GPL 许可证，与 GNU 工具整合形成 GNU/Linux |
| **1993**       | Debian 项目成立         | 标志性社区发行版之一，注重自由与稳定                              |
| **1994.03**    | Linux 1.0 发布        | 支持 TCP/IP 网络、虚拟内存和多用户，具备实用性                     |
| **1998**       | 企业支持 Linux          | IBM、HP 等大型企业宣布支持 Linux，加速其商业化                   |
| **2001**       | Linux 2.4 发布        | 增强 USB、RAID、多处理器支持                              |
| **2004**       | Ubuntu 发行           | 以 Debian 为基础，目标为“Linux for human beings”        |
| **2007**       | Android 使用 Linux 内核 | Google 开发 Android 移动操作系统，内核基于 Linux             |
| **2011**       | Linux 3.0 发布        | 内核版本号简化，持续增强硬件支持和性能                             |
| **2020**       | Linux 5.8 发布        | 超过 2800 万行代码，是史上最复杂的开源项目之一                      |
| **至今**         | 持续演进                | 在服务器、嵌入式、超算、云原生等领域广泛应用                          |

