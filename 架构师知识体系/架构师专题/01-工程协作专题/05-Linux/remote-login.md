---
title: 远程登录Linux系统
shortTitle: 
description: 
icon: 
cover: 
author: 流浪码客
isOriginal: true
sticky: false
star: false
date: 2025-07-20
category: 工程协作
tags:
  - Linux
---
# 远程登录Linux系统
Linux 主要用于服务器领域，这些服务器通常放在 IDC 机房中，因此我们一般通过网络远程登录。
## 安装并启用 SSH
> Linux 通过 `sshd` 服务实现远程登录，系统安装后该服务<u>默认开启并随系统启动</u>，无需额外配置。
* 默认端口：22
* 配置文件：`/etc/ssh/sshd_config`
**配置文件示例**
```shell
Port 11587                    # 修改默认端口
PermitRootLogin no            # 禁止 root 登录
PasswordAuthentication no     # 禁用密码登录
PubkeyAuthentication yes      # 启用公钥认证
AuthorizedKeysFile .ssh/authorized_keys  # 公钥文件路径
LoginGraceTime 2m             # 登录超时时间
MaxAuthTries 3                # 最大认证尝试次数
ClientAliveInterval 60        # 客户端存活检测间隔
ClientAliveCountMax 3         # 最大无响应次数
TCPKeepAlive yes              # 启用 TCP 保持连接
UseDNS no                     # 禁用 DNS 反查，加快登录
Banner /etc/issue.net         # 登录提示横幅（可选）
AllowUsers user1 user2        # 仅允许指定用户登录
Subsystem sftp /usr/libexec/sftp-server  # 启用 SFTP
```
### Ubuntu（22/24+）
```shell
sudo apt update
sudo apt install -y openssh-server   # 安装 OpenSSH 服务（ubuntu）
sudo systemctl enable --now ssh      # 启动并设置开机启动
sudo systemctl status ssh            # 查看服务状态，确保 active (running)
```
### CentOS 9 / Rocky 9
```shell
sudo dnf install -y openssh-server
sudo systemctl enable --now sshd
sudo systemctl status sshd
```
## SSH登录命令
**语法：`ssh -p 端口号 用户名@服务器IP`**
### 密码访问
* 设置：`PasswordAuthentication yes`，默认允许
```shell
sudo passwd joeljhou                 # 修改指定用户密码
ssh joeljhou@ubuntu24.orb.local      # 密码访问（ubuntu24.orb.local 可替换为ip地址）
```
### 密钥登录
* 设置：`PubkeyAuthentication yes`，默认允许
```shell
# 上传公钥
scp ~/.orbstack/ssh/id_ed25519.pub joeljhou@ubuntu24.orb.local:~/
# 登录远程服务器并执行
mkdir -p ~/.ssh
chmod 700 ~/.ssh
cat ~/id_ed25519.pub >> ~/.ssh/authorized_keys
chmod 644 ~/.ssh/authorized_keys
rm ~/id_ed25519.pub
# 密钥访问
ssh -i ~/.orbstack/ssh/id_ed25519 joeljhou@ubuntu24.orb.local
```
### 命令汇总
```shell
# OrbStack 虚拟机登陆命令
ssh -i ~/.orbstack/ssh/id_ed25519 -p 32222 joeljhou@ubuntu24@orb
ssh -i ~/.orbstack/ssh/id_ed25519 -p 32222 joeljhou@centos9@orb
ssh -i ~/.orbstack/ssh/id_ed25519 -p 32222 joeljhou@rocky9@orb
# 标准ssh命令
ssh joeljhou@localhost
ssh -i ~/.orbstack/ssh/id_ed25519 joeljhou@ubuntu24.orb.local
ssh -i ~/.orbstack/ssh/id_ed25519 joeljhou@centos9.orb.local
ssh -i ~/.orbstack/ssh/id_ed25519 joeljhou@rocky9.orb.local
```
