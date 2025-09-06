---
title: Linux 内存与磁盘管理
shortTitle:
description:
icon:
cover:
author: 流浪码客
isOriginal: true
sticky: false
star: false
date: 2025-09-04
category: 工程协作
tags:
  - Linux
---
# Linux 内存与磁盘管理
## 内存查看
### `free`：显示系统内存
```shell
$ free                            # 显示系统内存
	  total:总  used:使用  free:未使用  shared:共享  buff/cache:缓存  available:可用
Mem：  2002908   1057364      154056      182744          1069240         945544
Swap： 2097148    172544     1924604
$ free -k                         # 以 KB 显示（默认）
$ free -m                         # 以 MB 显示
$ free -g                         # 以 GB 显示
```
### `vmstat`：监控整体资源状态（CPU/内存/IO）
```shell
vmsta                             # 显示系统进程、内存、swap、I/O 和 CPU 使用情况
vmstat 2 5                        # 每2秒报告一次，共5次
# 输出示例
$ vmsta
procs ----------memory--------- ---swap-- -----io---- -system-- -------cpu------
 r b   交换 空闲 缓冲 缓存           si so    bi   bo    in cs    us sy id wa st gu
 1 0   208896 260092 29736 749208 25 98    883 545    552 5     5  2 93  0  0  0
# -----------------------------
# procs（进程）
# r   ← 等待运行的进程数（就绪队列长度）
# b   ← 不可中断睡眠的进程数（阻塞）
# -----------------------------
# memory（内存）
# 交换（swpd） ← 已使用 swap 空间（KB）
# 空闲（free） ← 空闲物理内存（KB）
# 缓冲（buff） ← 缓冲区占用内存（KB）
# 缓存（cache） ← 缓存占用内存（KB）
# -----------------------------
# swap（交换区 I/O）
# si  ← swap 换入内存速率（KB/s）
# so  ← swap 换出到 swap 的速率（KB/s）
# -----------------------------
# io（磁盘 I/O）
# bi  ← 块设备读速率（KB/s）
# bo  ← 块设备写速率（KB/s）
# -----------------------------
# system（系统）
# in  ← 每秒中断次数
# cs  ← 每秒上下文切换次数
# cpu（CPU 使用情况，百分比）
# -----------------------------
# cpu（CPU 使用情况，百分比）
# us  ← 用户态 CPU 占用
# sy  ← 内核态 CPU 占用
# id  ← 空闲 CPU 百分比
# wa  ← 等待 I/O 的时间百分比
# st  ← 被虚拟机偷取的 CPU 百分比
# gu  ← 未使用或特殊字段（依系统版本而异）
```
### `top`：实时进程与资源占用
```shell
$ top                             # 显示实时进程、内存、CPU等
PID   USER   PR    NI     VIRT   RES     SHR    %CPU     %MEM     TIME+  COMMAND
进程号 用户 优先级 调度优先值 虚拟内存 常驻内存 共享内存 CPU占比 内存占比 累计CPU时间 命令
526 joeljhou 20   0       14200   5492   3328 R   0.3    0.0    26:29.44   top  
top -i                            # 忽略空闲/僵尸进程
top -u username                   # 显示指定用户的进程
top -o %MEM                       # 按内存占用排序
top -o %CPU                       # 按CPU占用排序
top -Hp process_id                # 显示指定进程的所有线程信息
top -p $(pgrep -d ',' PNAME)      # 根据进程名提取 PID 并显示指定进程
```
### `htop`：增强版进程监控（交互/彩色/树形）
```shell
# 安装（Ubuntu）
sudo apt update
sudo apt install -y htop
htop --version
# -----------------------------
$ htop                            # top 增强版（彩色界面，可交互）
htop -u username                  # 显示指定用户的进程
htop -t                           # 树形视图，展示父子进程关系
htop -s sort_item                 # 按指定字段排序 (htop --sort help 查看可用项)
htop -d 50                        # 设置刷新间隔，50=5秒 (单位=0.1秒)
htop -h                           # 查看命令行帮助
```
## 磁盘空间查看
### `df`：磁盘分区使用情况
```shell
df                                # 查看所有分区使用情况
df -h                             # 人类可读形式（G/M/K）
df /home                          # 查看指定路径所在分区
df -i                             # 查看 inode 使用情况（文件/目录数量限制）
df -T                             # 显示文件系统类型
df -x tmpfs                       # 排除指定类型文件系统
```
[阮一峰-理解 inode](https://www.ruanyifeng.com/blog/2011/12/inode.html)
### `du`：目录/文件占用
```shell
du                                # 查看目录及子目录大小（默认 KB）
du -a                             # 包含隐藏的
du -b|k|m /home                   # 指定单位（B/K/M）
du -h /home                       # 人类可读形式（G/M/K）
du -s /home                       # 列出总和
du -ch ./*.txt                    # 查看当前目录下所有 .txt 文件大小，最后统计总和
du -h --max-depth=1 /home         # 递归深入1 层
du -ah --threshold=1G .[^.]* *    # 列出超过阈值的文件和目录
```
**`ls` 和 `du` 的区别：**
- `ls -lh` → 文件逻辑大小；**数据处理/传输**：看 `ls` 更直观。
-  `du -h` → 磁盘占用大小；**日常开发/运维**：更常用 `du` 看磁盘占用，尤其目录和日志管理。

## 磁盘分区/格式化/挂载/卸载
### `fdisk`：磁盘分区管理
```shell
sudo fdisk -l                     # 列出磁盘及分区
Disk /dev/sda：64 GiB，68719476736 字节，134217728 个扇区
Disk model: ubuntu-linux-24.
单元：扇区 / 1 * 512 = 512 字节
扇区大小(逻辑/物理)：512 字节 / 4096 字节
I/O 大小(最小/最佳)：4096 字节 / 4096 字节
磁盘标签类型：gpt
磁盘标识符：0E5F85DC-974E-414A-9110-4901627D049A

设备          起点      末尾      扇区  大小 类型
/dev/sda1     2048   2203647   2201600    1G EFI 系统
/dev/sda2  2203648 134215679 132012032 62.9G Linux 文件系统
# -----------------------------
# 查看磁盘设备信息
# ┌─ b : 块设备 (block device)
# ├─ rw- rw- --- : 权限 (owner/group/other)
# ├─ root disk : 所有者和所属组
# └─ 8,0 : 主/次设备号（8 表示 SCSI/SATA 磁盘，0 表示第一个磁盘）
$ ls -l /dev/sda*
brw-rw---- 1 root disk 8, 0  9月  6 22:32 /dev/sda   # 次设备号 0 → 整块磁盘
brw-rw---- 1 root disk 8, 1  9月  6 22:32 /dev/sda1  # 次设备号 1 → 第 1 个分区
brw-rw---- 1 root disk 8, 2  9月  6 22:32 /dev/sda2  # 次设备号 2 → 第 2 个分区
# -----------------------------
sudo fdisk /dev/sdaX              # 启动分区操作工具
<n>   # 创建一个 n 新分区
<d>   # 选择一个分区以 d 删除
<p>   # 查看 p 分区表
<w>   # 写入所做的更改
<q>   # 放弃所做的更改并 quit
<m>   # 打开帮助 menu
```
### 常见的文件系统
虽然已经完成分区，但磁盘分区 **还不能直接使用**。要让系统识别并存储文件，需要对每个分区进行 **格式化**——也就是安装文件系统。
- **Windows** 常用文件系统：`FAT32`、`NTFS`
-  **Linux（CentOS 7/8 默认）**：`XFS`，也可以选择 `ext3` 或 `ext4`
格式化完成后，分区才能正常读写数据。

| 系统                | 文件系统     | 特点                         | 适用场景             |
| ----------------- | -------- | -------------------------- | ---------------- |
| `Windows`         | `FAT32`  | 简单、兼容性好，单文件最大 4GB，分区最大 2TB | U盘、移动硬盘、兼容老系统    |
| `Windows`         | ==NTFS== | 支持大文件、权限控制、日志功能            | 系统盘、数据盘、现代应用     |
| `CentOS 6`        | `ext3`   | 稳定、支持日志，但性能一般              | 老版本 Linux 系统     |
| `CentOS 6/7`      | `ext4`   | 支持大文件、性能好、向下兼容 ext3        | 数据盘、现代 Linux 系统  |
| `CentOS 7/8`      | ==XFS==  | 高性能、支持大文件、日志功能             | 大文件存储、高性能服务器     |
| `Ubuntu`、`Debian` | ==ext4== | 默认文件系统，稳定、性能好              | 系统盘、数据盘、一般服务器和桌面 |
| `Ubuntu`、`Debian` | `XFS`    | 高性能、支持大文件、日志功能             | 大文件存储、企业级服务器     |
| `Ubuntu`、`Debian` | `Btrfs`  | 支持快照、压缩、校验、子卷              | 高级存储管理、快照备份、测试环境 |
### `mke2fs`/`mkfs.*`：创建 ext 文件系统
`mke2fs`、`mkfs.ext2`、`mkfs.ext3`、`mkfs.ext4` 这四个命令本质上是 **同一个程序的不同名字**。使用 `man` 查询其中任意一个命令时，看到的都是相同的帮助文档。这意味着它们的功能和选项大体相同，只是默认创建的文件系统类型不同。
```shell

mke2fs -t ext2 /dev/sdb1          # 创建 ext2 文件系统
mke2fs -t ext3 /dev/sdb1          # 创建 ext3 文件系统  
mke2fs -t ext4 /dev/sdb1          # 创建 ext4 文件系统（等价于 mkfs.ext4 /dev/sdb1）
# 常用选项：
-L <label>                        # 给分区设置标签名
-b <blocksize>                    # 指定块大小，默认 4096 字节，可选 1024/2048/4096
-i <inode-size>                   # 指定 inode 大小
-N <num-inodes>                   # 指定 inode 总数量（默认值可能不够）
-m <reserved-blocks-percentage>   # 保留给超级用户的磁盘百分比
-c                                # 格式化前检查设备是否有坏块（会显著降低速度）
-j                                # 创建 ext3 文件系统（使用 mkfs.ext3 可省略）
-t <type>                         # 指定文件系统类型，可选 ext2/ext3/ext4
```
### `mkfs.xfs`：创建 XFS 文件系统
`mkfs.xfs`用法有点特殊，你需要注意区分它和`mke2fs`。
```shell
mkfs.xfs /dev/sdb1                # 创建默认 XFS 文件系统
mkfs.xfs -b size=4096 -i size=512 /dev/sdb1
                                  # 指定块大小=4096 字节，inode大小=512 字节
mkfs.xfs -L mydata /dev/sdb1      # 设置卷标，给分区设置标签名
mkfs.xfs -f -b size=8192 /dev/sdb1# 强制覆盖已有文件系统
```
* 参考极客时间：https://time.geekbang.org/column/article/741028
### `e2label`：查看或修改分区标签
* 只支持 `ext` 格式的文件系统，而不支持 XFS 文件系统。
```shell
e2label /dev/sdb5 "label_name"    # 修改特定 ext 分区的卷标
e2label /dev/sdb5                 # 查看
```
### `mount`：挂载
```shell
显示所有挂载的文件系统：
mount

将设备挂载到目录：
mount path/to/device_file path/to/target_directory

创建特定目录（如果不存在），并将设备挂载到该目录：
mount [-m|--mkdir] path/to/device_file path/to/target_directory

为特定用户将设备挂载到目录：
mount [-o|--options] uid=user_id,gid=group_id path/to/device_file path/to/target_directory

将 CD-ROM 设备（文件类型为 ISO9660）挂载到 /cdrom（只读）：
mount [-t|--types] iso9660 [-o|--options] ro /dev/cdrom /cdrom

挂载 /etc/fstab 中定义的所有文件系统：
mount [-a|--all]

挂载由 /etc/fstab 中描述的特定文件系统（例如 /dev/sda1 /my_drive ext2 defaults 0 2 ）：
mount /my_drive

将一个目录挂载到另一个目录：
mount [-B|--bind] path/to/old_dir path/to/new_dir
```
### `/etc/fstab` 配置

### `blkid`：获取各分区的UUID

### `umount`：卸载
## 交换空间管理
- 6.6.1 查看 Swap 分区
- 6.6.2 创建 Swap 文件
#### 6.7 文件系统与存储扩展
- ext4 文件系统
- 磁盘配额（Quota）
- 软件 RAID
- 逻辑卷管理（LVM）
#### 6.8 系统状态监控
- 内存使用情况
- 磁盘 I/O 状态
- 综合性能查看
