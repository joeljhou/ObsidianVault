---
title: Linux 用户与文件权限管理
shortTitle:
description:
icon:
cover:
author: 流浪码客
isOriginal: true
sticky: false
star: false
date: 2025-09-02
category: 工程协作
tags:
  - Linux
---
# Linux 用户与文件权限管理
## 用户管理命令
### `useradd`：新建用户
```shell
useradd alice                     # 创建普通用户
useradd -u 1500 bob               # 指定 UID=1500
useradd -s /bin/bash charlie      # 指定登录 Shell（配置 /etc/default/useradd）
useradd -G sudo,docker diana      # 加入 sudo 和 docker 组
useradd -m alice                  # 自动生成家目录（拷贝 /etc/skel 模板）
useradd -m -k template_dir bob    # 使用指定模板目录创建家目录
useradd -r systemuser             # 创建系统用户（UID<1000，普通用户 UID≥1000）
```
### `userdel`：删除用户
```shell
userdel alice                     # 仅删账号（适合停用账号但保留数据）
userdel -r alice                  # 删账号并清理家目录、邮件缓存
userdel -R /mnt/ubuntu_root alice # 在挂载的系统根目录下操作（如 chroot/容器/救援系统）
```
### `usermod`：修改用户属性
```shell
usermod -l newname alice          # 修改用户名（alice → newname）
usermod -u 1600 bob               # 修改 UID
usermod -s /bin/sh charlie        # 修改默认 Shell
grep '^charlie:' /etc/passwd      # 验证修改结果，查看用户 Shell
usermod -aG sudo,docker diana     # 追加附加组
usermod -G devs,qa,ops alice      # 设置附加组（覆盖原组）
usermod -rG sudo,docker alice     # 移出指定组
usermod -d /new/home bob          # 修改家目录（不迁移文件，ls -a查看隐藏文件🫥）
usermod -m -d /new/home bob       # 修改并迁移家目录
usermod -L alice                  # 锁定账号（禁止登录）
usermod -U alice                  # 解锁账号
```
### `passwd`：修改用户密码
```shell
passwd                            # 修改当前用户密码（交互式）
passwd alice                      # 修改指定用户密码
passwd -S alice                   # 获取指定用户状态（P=已设置密码，L=锁定，NP=无密码）
passwd -d alice                   # 删除用户密码（可直接登录）
passwd -e alice                   # 强制用户下次登录修改密码
# 非交互式修改密码（脚本自动化）
echo "newpwd" | passwd --stdin alice   # RedHat/CentOS 支持
echo "alice:newpwd" | sudo chpasswd    # Ubuntu/Debian 推荐
yes "root" | passwd                    # 临时自动化（不安全，会无限输出）
```
### `chage`：用户密码过期管理
```shell
chage --list alice                # 列出用户密码信息（-l）
chage --maxdays 10 alice          # 设置密码10天过期（-M）
chage --maxdays -1 alice          # 禁用密码过期
chage --expiredate 2025-12-31 alice  # 设置账户到期日期（-E）
chage --expiredate -1 alice       # 重新启用账户
chage --lastday 0 alice           # 强制用户下次登录修改密码（-d）
```
## 提权与身份切换
### `su`：切换用户
```shell
su                                # 切 root（需 root 密码）
su alice                          # 切指定用户（需该用户密码）
su - alice                        # 切指定用户并加载环境（完整登录）
su - alice -c "id -a"             # 以指定用户执行命令
```
### `sudo`：临时提权执行命令
```shell
sudo less /var/log/syslog         # 以 root 执行命令
sudo -e /etc/fstab                # 以 root 打开文件（用默认编辑器）
sudo -u user -g group id -a       # 指定用户/组运行命令
sudo !!                           # 重新执行上一条命令并加 sudo（⚠️仅Bash、Zsh有效）
sudo -i                           # 模拟 root 登录（加载.profile、.bash_profile等）
sudo -s                           # root shell（不改环境）
sudo -i -u user                   # 模拟指定用户完整登录
sudo -ll                          # 详细版 sudo 权限清单
```
### `visudo`：sudo 用户/组配置
```shell
sudo visudo                       # 安全编辑 /etc/sudoers 文件
sudo visudo -c                    # 检查 sudoers 配置是否存在错误
sudo EDITOR=nano visudo           # 使用指定编辑器（如 nano）打开 sudoers
visudo -V                         # 查看 visudo 版本
```
**`/etc/sudoers`文件概览**
```shell
# 注意：此文件必须使用 'visudo' 命令以 root 身份编辑
# 建议把本地自定义内容放在 /etc/sudoers.d/ 目录，而不是直接修改本文件
# 具体 sudoers 语法和规则请参考 man 页面：man sudoers

# ==============================
# 1. 默认设置 (Defaults)
# ==============================
Defaults        env_reset         # 重置环境变量，避免继承用户环境变量造成安全风险
Defaults        mail_badpass      # 密码错误时发送邮件通知
Defaults        secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"                              # 设置安全 PATH，确保 sudo 执行命令路径安全
Defaults        use_pty           # 使用伪终端，提高安全性（修复 CVE-2005-4890）

# ==============================
# 2. 保留用户偏好环境变量
# ==============================
#Defaults:%sudo env_keep += "http_proxy https_proxy ftp_proxy all_proxy no_proxy"                                                  # 保留用户代理设置
#Defaults:%sudo env_keep += "EDITOR"                       # 保留用户指定编辑器
#Defaults:%sudo env_keep += "GREP_COLOR"                   # 保留 grep 高亮显示配置
#Defaults:%sudo env_keep += "GIT_AUTHOR_* GIT_COMMITTER_*" # 保留 git 相关环境变量
#Defaults:%sudo env_keep += "SSH_AGENT_PID SSH_AUTH_SOCK"  # 保留 SSH 代理信息
#Defaults:%sudo env_keep += "GPG_AGENT_INFO"               # 保留 GPG 代理信息
#Defaults:%sudo env_keep += "EMAIL DEBEMAIL DEBFULLNAME"   # 保留用户邮箱信息

# ==============================
# 3. 别名定义（可选）
# ==============================
# 主机别名
# Host_Alias FILESERVERS = fs1, fs2
# Host_Alias MAILSERVERS = smtp, smtp2
# 用户别名
# User_Alias ADMINS = jsmith, mikem
# 命令别名
# Cmnd_Alias SYSTEM_CMDS = /bin/systemctl, /bin/journalctl

# ==============================
# 4. 用户与组权限定义（用户 主机=命令）
# ==============================
root    ALL=(ALL:ALL) ALL         # root 用户在所有主机上可以以任何用户身份执行任何命令
%admin  ALL=(ALL) ALL             # admin 组成员可获得 root 权限
%sudo   ALL=(ALL:ALL) ALL         # sudo 组成员可执行任何命令

# ==============================
# 5. 包含自定义 sudoers 文件
# ==============================
@includedir /etc/sudoers.d        # 引入 /etc/sudoers.d/ 下的所有配置文件
```
## 用户和用户组配置
### `/etc/passwd`：用户账户配置
```shell
$ sudo cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
│    │ │ │ │    │       └── 登录 Shell（用户默认使用的命令行解释器）
│    │ │ │ │    └── 用户家目录（登录后默认所在目录）
│    │ │ │ └── 用户描述信息（root，可填写姓名或说明）
│    │ │ └── 组ID（GID，所属主组的编号）
│    │ └── 用户ID（UID，用户唯一编号）
│    └── 密码占位符（实际密码存储在 /etc/shadow）
└────── 用户名（login name）
```
### `/etc/shadow`：用户密码配置
```shell
$ sudo cat /etc/shadow
用户名:加密密码:修改日期:最小有效期:最大有效期:过期提醒天数:保留:保留
root:$y$j9T$YsY……:20333:0:99999:7:::
│    │             │    │  │ │  └──── 账号失效日期（天数，从1970-01-01开始）
│    │             │    │  │ └────── 密码过期前警告天数
│    │             │    │  └────── 密码最大有效期（天）
│    │             │    └──────── 密码最小有效期（天）
│    │             └─────────── 上次修改密码的日期（从1970-01-01开始的天数）
│    └──────────────────────── 加密密码（hash值）
└──────────────────────────── 用户名
```
### `/etc/group`：用户组配置
```shell
$ cat /etc/group
root:x:0:
│    │ │ └── 组内用户列表（逗号分隔，如 alice,bob）
│    │ └── 组ID（GID）
│    └── 密码占位符（实际密码存储在 /etc/gshadow）
└────── 组名
```
### `id`：查看用户所有者和组
```shell
id                                # 显示当前用户 UID、GID 及所属组
id -un                            # 用户名
id -u                             # UID
id -gn                            # 用户默认组名
id -g                             # GID
# 查看任意用户的 UID、GID 及所属组
$ id joeljhou
uid=501(joeljhou) gid=501(joeljhou) groups=501(joeljhou),4(adm),27(sudo),44(video),50(staff),67278(orbstack)
```
### `groups`：查看用户组信息
```shell
groups                            # 当前用户所属的所有组
groups alice bob                  # 查看指定用户的组信息
```
## 文件权限管理命令
> Linux 是一个 **多用户、多任务** 的操作系统。为了实现 **权限管理与隔离**，系统为每个文件和目录都设置了 **所有者 (owner)** 和 **所属组 (group)**，以及相应的访问权限。
### `ls -l`：查看文件权限
```shell
$ ls -l /etc/passwd
-rw-r--r--  1 root root  1312 7月 23 17:09 /etc/passwd
│             │    │     │                 └── 文件名
│             │    │     └───── 文件大小（字节）
│             │    └─────────── 文件所属组 (group)
│             └──────────────── 文件所有者 (user)
└────────────────────────────── 权限部分
        第1位：- 文件，d 目录，l 软链接；b 块设备，c 字符设备，s 套接字，p 命名管道，D 门
        后9位分为三组，每组三位依次表示（可读/写/执行）：
        Owner rwx / Group rwx / Others rwx）
```
### `chmod`：修改文件权限
```shell
### `chmod`：修改文件权限
# 符号法（u=用户，g=组，o=其他，a=所有；r=读，w=写，x=执行，X=仅目录执行）
chmod u+x file                    # 授予所有者执行权限
chmod u+rw file_or_directory      # 授予所有者读写权限
chmod a+rx file                   # 授予所有用户读和执行权限
chmod g-x file                    # 移除用户组执行权限
chmod o=g file                    # 授予其他用户（非文件所有者组）与[g]组相同权限
chmod o= file                     # 移除其他用户所有权限
chmod -R g+w,o+w directory        # 递归给组和其他用户加写权限
chmod -R a+rX directory           # 递归：文件加读，目录加执行权限
# 数字法（r=4，w=2，x=1；依次为 u/g/o 三组）
chmod 644 file                    # rw-r--r-- （所有者读写，组和其他只读）
chmod 600 file                    # rw------- （仅所有者读写，常用于私钥）
chmod 755 file                    # rwxr-xr-x （所有者读写执行，其他读执行）
chmod 700 directory               # rwx------ （仅所有者有全部权限）
chmod -R 755 /var/www             # 递归修改目录及内容权限
```
### `chown`：修改所有者和组
```shell
chown user file_or_directory      # 修改所有者
chown user:group file_or_directory# 修改所有者和组
chown user: file_or_directory     # 修改所有者并清空组
chown -R user directory           # 递归修改目录及内容所有者
chown -h user symlink             # 修改符号链接本身所有者
chown --reference=ref_file file   # 按参考文件修改所有者和组
```
### `chgrp`：修改所属组
```shell
chgrp group file_or_directory     # 修改所属组
chgrp -R group directory          # 修改目录下文件所属组（递归）
chgrp -h group symlink            # 修改符号链接所属组
chgrp --reference=ref_file file   # 按参考文件修改文件所属组
```
### `umask`：默认权限掩码
文件默认最大权限 `666`，目录默认最大权限 `777`，再减去 `umask` 的值
```shell
umask                             # 显示当前掩码（八进制，如 0022）
umask -S                          # 以符号方式显示（如 u=rwx,g=rx,o=rx）
umask a+r                         # 符号方式修改掩码（等价于=0022）
umask 022                         # 常用：新文件 644，新目录 755
umask 077                         # 安全：新文件 600，新目录 700
umask 002                         # 协作：新文件 664，新目录 775
```
### `chattr`：修改文件特殊属性
* 修改文件或目录的特殊属性，增强安全性，即使是 root 用户也受限制
```shell
chattr +i file_or_directory       # 设置文件不可修改（无法编辑，删除，重命名，移动）
chattr -i file.txt                # 移除不可修改属性
chattr -R +i directory            # 设置文件不可修改（递归目录）
chattr +F /path/to/dir            # 将目录标记为不区分大小写（文件系统支持）
chattr +a logfile.log             # 设置文件为仅追加写入
chattr -a logfile.log             # 移除仅追加写入属性
```
### `lsattr`：查看文件特殊属性
* 查看文件或目录的增强型属性（`chattr` 设置的属性）
```shell
lsattr                            # 显示当前目录下所有文件的属性
lsattr path                       # 显示指定路径文件属性
lsattr -R                         # 递归显示当前目录及子目录文件属性
lsattr -a                         # 显示当前目录中所有文件属性，包括隐藏文件
lsattr -d                         # 仅显示目录属性，不显示目录下的文件
```


