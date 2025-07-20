---
title: 搭建Linux环境
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
# 搭建Linux环境
## 搭建方式
Linux 是互联网应用的基础，广泛用于服务器与服务搭建。常见的环境搭建方式有：**云服务器、虚拟机和容器化**，它们虽都提供可用的 Linux 环境，但在搭建方式、资源消耗和适用场景上各有区别。
### ☁️ 云服务器
> 通过云服务平台<u>租用远程 Linux 主机</u>，开箱即用，适合生产部署和远程协作。

**🌍 国际**
1. **[亚马逊 AWS](https://aws.amazon.com/)** 
2. **[微软 Azure](https://azure.microsoft.com/)
3. **[谷歌云 (Google Cloud Platform)](https://cloud.google.com/)** 
4. **[甲骨文云 (Oracle Cloud)](https://www.oracle.com/cloud/)** 
5. **[IBM 云](https://www.ibm.com/cn-zh/cloud)**
---
**🇨🇳 中国**
1. **[阿里云](https://www.aliyun.com/)** 
2. **[腾讯云](https://cloud.tencent.com/)
3. **[华为云](https://www.huaweicloud.com/)**  
4. **[百度云](https://cloud.baidu.com/)** 
5. **[七牛云](https://www.qiniu.com/)** 
### 💻 虚拟机
> 通过虚拟机软件直接在本地安装运行 Linux，环境独立，占用资源大，适合学习使用。

**常见虚拟化软件**
1. **[VMware](https://www.vmware.com/)**
2. **[VirtualBox](https://www.virtualbox.org/)** 
3. **[Hyper-V](https://learn.microsoft.com/en-us/virtualization/hyper-v-on-windows/)**
4. **[KVM](https://www.linux-kvm.org/)** 
5. **[Parallels Desktop](https://www.parallels.com/)**
### 🐳 容器化
> 基于容器技术，共享宿主机内核，启动速度快，资源占用少。

**主流容器工具**
1. [Docker](https://www.docker.com/)
2. [OrbStack](https://orbstack.dev/)
3. [Podman](https://podman.io/)
4. [Kubernetes](https://kubernetes.io/)
## 版本选择
### 🧬 内核版本
官方内核站点：[https://www.kernel.org/](https://www.kernel.org/)
### 🏷️ Linux 发行版本
> “发行版”是在内核基础上，附带了包管理器、系统服务、桌面环境、文档和社区支持的一整套操作系统。

**[Linux发行版时间线](https://upload.wikimedia.org/wikipedia/commons/a/ad/2023_Linux_Distributions_Timeline.svg)**
#### 体系分类（三大主流）
```mermaid
graph LR
    B[Linux内核]
    
    B --> C[**Debian**<br>稳定、社区支持好]
    C --> C1[Ubuntu<br>服务器/桌面常用]
    C --> C2[Kali<br>安全测试工具丰富]
    
    B --> D[**Red Hat（RHEL）**<br>企业级，稳定付费]
    D --> D1[Fedora<br>前沿技术测试平台]
    D --> D2[CentOS<br>RHEL兼容（已转为流）]
    D --> D3[Rocky Linux<br>CentOS替代]

    B --> E[**Arch Linux**<br>滚动更新，高自由度]
    E --> E1[Manjaro<br>适合桌面用户，简化配置]

    B --> F[独立体系<br>自主开发，特色鲜明]
    F --> F1[openSUSE<br>企业与社区兼顾]
    F --> F2[Gentoo<br>源码编译，极致定制]
    F --> F3[Alpine<br>极简安全，容器首选]

    style C fill:#dff,stroke:#09c,stroke-width:2px
    style D fill:#ffd,stroke:#c90,stroke-width:2px
    style E fill:#dfd,stroke:#090,stroke-width:2px
    style F fill:#fdd,stroke:#c33,stroke-width:2px

    click B "https://linuxkernel.org" "Linux内核官网"
    click C "https://www.debian.org" "Debian官网"
    click C1 "https://ubuntu.com" "Ubuntu官网"
    click C2 "https://www.kali.org" "Kali官网"
    click D "https://www.redhat.com" "Red Hat官网"
    click D1 "https://getfedora.org" "Fedora官网"
    click D2 "https://www.centos.org" "CentOS官网"
    click D3 "https://rockylinux.org" "Rocky Linux官网"
    click E "https://archlinux.org" "Arch Linux官网"
    click E1 "https://manjaro.org" "Manjaro官网"
    click F1 "https://www.opensuse.org" "openSUSE官网"
    click F2 "https://www.gentoo.org" "Gentoo官网"
    click F3 "https://alpinelinux.org" "Alpine官网"
```
**✅ 建议学习顺序：**
1. **Ubuntu 24.04 LTS**：入门基础命令、目录结构、服务管理等。
2. **Rocky Linux 9**：进阶企业部署标准，理解 RHEL/Debian 差异。
#### 包管理器对比
```mermaid
graph LR

PackageMgr[包管理器]

PackageMgr --> APT[**APT** - dpkg]
PackageMgr --> RPM[**DNF** / **YUM** / RPM]
PackageMgr --> Pacman[Pacman]
PackageMgr --> Zypper[Zypper]
PackageMgr --> Portage[Portage]
PackageMgr --> apk[apk]

APT --> DebianFamily[**Debian系**<br>Debian, Ubuntu, Kali Linux]
RPM --> RedHatFamily[**Red Hat系**<br>RHEL, Fedora, CentOS, Rocky Linux]
Pacman --> ArchFamily[**Arch系**<br>Arch Linux, Manjaro]

Zypper --> SUSE[openSUSE]
Portage --> Gentoo[Gentoo]
apk --> Alpine[Alpine]

style APT fill:#dff,stroke:#09c,stroke-width:2px
style RPM fill:#ffd,stroke:#c90,stroke-width:2px
style Pacman fill:#dfd,stroke:#090,stroke-width:2px
style Zypper fill:#fdd,stroke:#c33,stroke-width:2px
style Portage fill:#fdd,stroke:#c33,stroke-width:2px
style apk fill:#fdd,stroke:#c33,stroke-width:2px
```
#### 桌面环境对比
```mermaid
graph LR
    DE[桌面环境]

    DE --> GNOME[**GNOME**<br>现代、易用]
    DE --> KDE[KDE Plasma<br>美观、功能丰富]
    DE --> XFCE[XFCE<br>轻量级，稳定]
    DE --> LXDE[LXDE/LXQt<br>超轻量]
    DE --> MATE[MATE<br>GNOME 2 继承者]
    DE --> Cinnamon[Cinnamon<br>Linux Mint默认]

    GNOME --> G1[Ubuntu默认环境]
    KDE --> K1[KDE Neon, openSUSE默认]
    XFCE --> X1[Xubuntu, Manjaro XFCE]
    LXDE --> L1[Lubuntu]
    MATE --> M1[Ubuntu MATE]
    Cinnamon --> C1[Linux Mint]

    style GNOME fill:#dff,stroke:#09c,stroke-width:2px
    style KDE fill:#ffd,stroke:#c90,stroke-width:2px
    style XFCE fill:#dfd,stroke:#090,stroke-width:2px
    style LXDE fill:#fdd,stroke:#c33,stroke-width:2px
    style MATE fill:#ddf,stroke:#06c,stroke-width:2px
    style Cinnamon fill:#fcf,stroke:#909,stroke-width:2px

    click GNOME "https://www.gnome.org" "GNOME官网"
    click KDE "https://kde.org" "KDE官网"
    click XFCE "https://xfce.org" "XFCE官网"
    click LXDE "https://lxde.org" "LXDE官网"
    click MATE "https://mate-desktop.org" "MATE官网"
    click Cinnamon "https://linuxmint.com" "Cinnamon官网"
```
