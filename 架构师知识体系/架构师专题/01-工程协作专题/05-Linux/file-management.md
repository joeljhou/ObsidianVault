---
title: Linux 文件与目录管理
shortTitle:
description:
icon:
cover:
author: 流浪码客
isOriginal: true
sticky: false
star: false
date: 2025-07-23
category: 工程协作
tags:
  - Linux
---
# Linux 文件与目录管理
> “万物皆文件”（Everything is a file）是 Linux 系统的核心设计理念，源自 UNIX 系统的哲学思想。该理念最早由《The UNIX Programming Environment》一书提出，强调所有资源（如设备、进程、网络连接等）都通过文件接口统一访问。
>
> —— *Kernighan, B. W., & Pike, R. (1984).* **The UNIX Programming Environment**. Prentice Hall.

**记住一句经典的话：==在 Linux 世界里，一切皆文件！！==**
## Linux 系统目录结构
Linux 文件系统是以根目录 `/` 为起点的树状结构，所有文件和目录都从这里开始。
```shell
# 查看根目录内容
$ ls -1F /
bin@               # 常用二进制命令（如 ls、cp、mv）
sbin@              # 系统管理命令（如 reboot、fdisk）
etc/               # 配置文件目录（如 passwd、hosts、network）
home/              # 普通用户主目录（如 /home/parallels）
root/              # 管理员 root 的主目录
boot/              # 启动文件（如内核 vmlinuz、引导程序 GRUB）
lib@               # 基础共享库，供 bin/sbin 使用
usr/               # 用户级程序与库，类似 Windows 的 Program Files
var/               # 可变数据目录（如日志 /var/log、缓存、邮件等）
dev/               # 设备文件（如硬盘 /dev/sda），类似“设备管理器”
proc/              # 虚拟文件系统，动态映射进程与内核信息（如 /proc/cpuinfo）
sys/               # 虚拟文件系统，内核驱动与硬件交互接口（如热插拔、设备属性）
tmp/               # 临时文件目录（重启后清空）
run/               # 运行时数据（如 PID、Socket），挂载在 tmpfs 内存中
srv/               # 服务数据目录（如 FTP、HTTP 站点数据）
opt/               # 第三方软件安装目录（如 /opt/google/chrome/）
media/             # 自动挂载点（如 U 盘、光驱），由桌面环境自动挂载
mnt/               # 手动挂载点，管理员用于挂载临时设备
lost+found/        # 文件系统异常时 fsck 恢复的碎片文件存放处（ext 专用）
```
##  通配符与模式匹配
通配符用于匹配文件或目录名，实现批量操作、过滤和管理。
```shell
# ========= 常用通配符 =========
# 星号 * ：匹配任意数量字符（包括零个字符）
ls *.txt           # 匹配当前目录下所有以 .txt 结尾的文件
ls file*           # 匹配以 file 开头的文件，如 file1、file_backup
# 问号 ? ：匹配任意单个字符
ls file?.txt       # 匹配 file1.txt、fileA.txt，但不匹配 file10.txt
# 中括号 [] ：匹配括号内任意一个字符
ls file[12].txt    # 匹配 file1.txt 或 file2.txt
ls file[a-z].txt   # 匹配 filea.txt ~ filez.txt
ls file[!0-9].txt  # 匹配 file 后不是数字的文件
# 大括号 {} ：花括号扩展，可匹配多个指定字符串
ls file{1,2,3}.txt # 匹配 file1.txt、file2.txt、file3.txt
ls {a,b,c}.txt     # 匹配 a.txt、b.txt、c.txt
# 反斜杠 \ ：转义特殊字符，防止通配符展开
ls \*.txt          # 匹配名为 *.txt 的文件，而不是所有 .txt 结尾的文件
# ========= 实际使用示例 =========
rm *.tmp                         # 删除当前目录所有以 .tmp 结尾的文件
cp file{1,2,3}.txt /backup/      # 复制多个文件到备份目录
mv [a-z]* /home/user/documents/  # 移动所有以字母开头的文件
ls [!.]*                         # 列出所有非隐藏文件（排除以 . 开头的文件）
wc -l *.log                      # 统计当前目录下所有日志文件的行数
```
💡 **小技巧**：
- 如果你想测试 “shell 不展开通配符”，可以用 `echo *.txt` vs `echo \*.txt` 看效果。
- 在脚本里处理特殊文件名时，通常要用转义或引号避免意外展开。
## 文件目录命令
### `cd`：切换目录
```shell
cd /                              # 进入根目录
cd                                # 进入当前用户主目录
cd ~                              # 同上
cd .                              # 刷新当前目录（无实际变动）
cd ..                             # 返回上一级目录
cd -                              # 返回上一次所在目录
cd /usr/local                     # 切换到指定路径
```
### `pwd`：显示路径
```shell
pwd                               # 显示当前路径
pwd -L  [-L|--logical]            # 显示逻辑路径（默认）
pwd -P  [-P|--physical]           # 显示物理路径（解析符号链接）

# 示例
$ cd /bin
$ pwd -P
/usr/bin
```
### `ls`：列出目录中的文件
```shell
ls                                # 简单列出文件
ls -1                             # 每行一个文件
ls -a [-a|--all]                  # 显示所有文件（含隐藏）
ls -F [-F|--classify]             # 文件类型标记（目录/，可执行*，链接@，管道|，套接字=）
ls -l [-la|--all -l]              # 显示详细信息（权限，所有者，大小，修改时间）
ls -la                            # 详细信息 + 隐藏文件
ls -lh [-lh|-l --human-readable]  # 人类可读大小（KiB, MiB, GiB）
ls -lS [-lSR|-lS --recursive]     # 按大小排序（大到小）
ls -lSr                           # 大小排序（小到大）
ls -lt [-ltr|-lt --reverse]       # 按修改时间排序（新到旧）
ls -ltr                           # 修改时间排序（旧到新）
ls -d */                          # 只列出目录
ls -l | grep '^d'                 # 过滤仅显示目录
# 别名
ll                                # 通常预设为 ls -l
```
###  `mkdir`：创建目录（并设置权限）
```shell
mkdir path/to/dir1                # 创建单个目录
mkdir dir1 dir2 ...               # 创建多个目录
mkdir -p path/to/dirname          # 递归创建多级目录
mkdir -p ./{a,b}/{x,y,z}          # 递归创建多个嵌套目录
mkdir -m 755 dirname              # 创建目录并设置权限
```
### `touch`：创建文件（并更新时间）
```shell
touch path/to/file1 ...           # 创建文件/并更新访问(a)和修改(m)时间为“当前时间”
touch file{1..10}                 # 批量创建文件（file1~file10）
touch file{a..z}                  # 批量创建文件（filea~filez）
touch -c -a|-m file1 ...          # 更新访问(a)和修改(m)时间为“当前时间”，不创建新文件
touch -t YYYYMMDDHHMM.SS file1    # 更新访问(a)和修改(m)时间为“指定时间”
touch -r ref a b ...              # 使用文件ref的时间更新到其它文件
touch -c -r ref a b ...           # 使用文件ref的时间更新到其它文件，不创建新文件
```
### `rmdir`：仅删除空目录
```shell
rmdir path/to/dirname             # 删除空目录（目录必须为空）
rmdir -p path/to/dirname          # 递归删除目录及其空的父目录（适用于嵌套空目录）
```
### `rm`：删除
```shell
rm path/to/file1 ...              # 删除文件
rm -f file1 file2 ...             # 强制删除文件，忽略不存在文件
rm -i file1 file2 ...             # 交互式删除（防止误操作）
rm -v file1 file2 ...             # 显示删除详细过程
rm -r dir1/ dir2/ ...             # 递归删除目录下所有内容
rm -d empty_dir/                  # 安全删除空目录（目录必须为空）
rm -rf dir1/ dir2/ ...            # 强制递归删除文件或目录（⚠️慎用）
```
### `cp`：复制
```shell
# cp 命令简介：
# source：源文件或目录
# target：目标文件或目录
-------------------------
# 基础文件复制
cp source_file target_file        # 复制文件
cp -v source_file target_file     # 复制文件，显示详细过程
cp -i *.txt new.txt               # 交互式覆盖（防止误操作）
# 目录复制（递归）
cp -r source_dir/ target_dir/     # 递归复制目录
cp -vr source_dir/ target_dir/    # 递归复制目录，并显示详细过程
# 归档复制（保留元数据）
cp -p source_file target_file     # 归档复制文件（保留权限和时间戳等，不包括符号链接）
cp -a source_dir/ target_dir/     # 归档复制目录（保留所有元数据和符号链接）
# 批量复制
cp -t destination_dir/ file1 file2 ...                 # 复制多个文件到目标目录
find . -name "*.log" | xargs cp -t /var/log/backup/    # 配合 xargs 使用，备份日志
# 符号链接与目录结构
cp -L link target_dir/            # 复制符号链接指向的目标文件
cp --parents ./src/file.txt dir/  # 保留目录结构复制（dir/src/file.txt）
# 覆盖控制（目录需配合 -r 或 -a 使用）
cp -u source_file target          # 仅当目标不存在或比源旧时才复制
cp -f source_file target          # 强制覆盖已存在目标文件（不提示）
cp -n source_file target          # 目标存在时不覆盖（不提示）
```
### `mv`：移动/重命名
```shell
mv old_file new_file              # 重命名文件（覆盖）
mv old_dir new_dir                # 重命名目录
mv file existing_dir/             # 移动文件，到目标目录
mv file1 file2 ... existing_dir/  # 移动多个文件，到目标目录
mv old_dir existing_dir/          # 移动目录，到指定目录（嵌套移动）
# 常用选项说明
mv -f old_file new_file           # 强制覆盖（默认行为）
mv -i old_file new_file           # 交互式覆盖（防止误操作）
mv -n old_file new_file           # 目标存在时不覆盖
mv -v old_file new_file           # 覆盖文件，显示详细过程
# 批量移动 /var/log 目录下的 *.log 文件到目标目录 existing_dir/
# 使用 -print0 和 xargs -0 组合，确保文件名中包含空格或特殊字符时命令依然正常执行
# mv -t 指定目标目录，方便批量移动文件
find /var/log -type f -name '*.log' -print0 | xargs -0 mv -t existing_dir/
```
## 文档查看命令
### `cat`：查看文件内容
```shell
cat /etc/hosts                    # 查看文件内容
cat part1 part2 > merged.txt      # 合并文件（覆盖）
cat part1 part2 >> merged.txt     # 合并文件（追加）
cat - > notes.txt                 # 从标准输入写入（Ctrl+D结束）
cat - >> notes.txt                # 从标准输入追加（Ctrl+D结束）
cat -n ~/.bashrc                  # 显示内容并带行号
# 🔎 显示不可打印字符和空白字符
cat [-vte | --show-nonprinting -t -e] 路径/到/文件
# 各选项说明：
# -v：显示控制字符（如响铃 \x07 → ^G，回车 \x0D → ^M，换页 \x0C → ^L，删除 \x7F → ^?，非 ASCII 显示为 M- 前缀）
# -t：将制表符 Tab 显示为 ^I（如 A\tB → A^IB，便于识别缩进）
# -e：行末添加 $ 并启用 -v（如 "abc\n" → abc$，用于标识换行和行结束）
# -A ：等价于 -vte

# 示例：创建包含特殊字符的文件
echo -e "Tab:\tEnd\nCR:\rLine\nDel:\x7F\nBell:\x07\n中文：中" > test.txt
$ cat -vte test.txt
Tab:^IEnd$             # Tab 显示为 ^I，行末有 $
CR:^MLine$             # 回车 \x0D 显示为 ^M
Del:^?$                # 删除符 \x7F 显示为 ^?，行末有 $
Bell:^G$               # 响铃符 \x07 显示为 ^G
中文：M-E M-4 M-B ...$  # 非 ASCII 字节（UTF-8 编码的“中”）逐字节显示为 M- 开头
```
### `tac`:  逆序查看文件
```shell
tac /etc/passwd                          # 按行倒序输出
tac /etc/hosts /etc/services             # 多文件依次倒序
cat /etc/hosts | tac                     # 从标准输入逆序
grep 'session' /var/log/auth.log | tac   # 筛选后再逆序
echo "apple,banana,orange" | tac -s ','  # 以逗号分隔倒序（--separator）
echo "A1B2C3D4E5" | tac -rs '[0-9]+'     # 正则分隔倒序（--regex）
echo "L1--L2--L3--L4" | tac -bs '--'     # 分隔符在前（--before）
```
### `more`：简单分页
```shell
# 生成一个测试文件
seq 1 100 > numbers.txt
more numbers.txt                  # 分屏显示 (--More--(xx%) 表示进度)
more +50 numbers.txt              # 从第 50 行开始显示
# 操作说明🕹️：
# Enter(回车)     → 下一行
# Space(空格键)   → 下一页
# Ctrl+F         → 向下翻一屏（与空格类似）
# Ctrl+B         → 向上翻一屏
# /关键字<Enter>  → 搜索字符串（例如 /1）
# n              → 跳到下一页匹配项（向下👇）
# q              → 退出
```
### `less`：灵活分页
```shell
less 路径/到/文件                   # 打开文件
# 操作说明：
# <j> / <k>      → 向下/向上一行
# <Space> / <b>  → 向下/向上翻页
# <g> / <G>      → 跳到文件开头/末尾
# /关键字<Enter>  → 搜索字符串（向下👇）
# ?关键字<Enter>  → 搜索字符串（向上👆）
# <n> / <N>      → 跳到下一页匹配项（同方向/反方向）
```
### `head`：查看文件开头
```shell
head filename                     # 默认前 10 行
head -n 20 filename               # 前 20 行
head -c 20 filename               # 前 20 字节
head -n -10 filename              # 除去最后 10 行
head -c -20 filename              # 除去最后 20 字节
```
### `tail`：查看文件结尾
```shell
tail filename                     # 默认最后 10 行
tail -n 20 filename               # 最后 20 行
tail -c 20 filename               # 最后 20 字节
tail -n +M filename               # 从第 M 行开始直到末尾
tail -c +M filename               # 从第 M 字节开始直到末尾
# 实时查看文件
tail -f file.log                  # 实时跟踪文件变化（常用于日志）
tail -F file.log                  # 类似 -f，文件被替换/重建时继续跟踪
tail -n 20 -f file.log            # 先显示最后 20 行，再实时跟踪
tail -n 20 -s 2 -f file.log       # 每隔 2 秒刷新一次
```
### `wc`：文件统计
```shell
echo -e "Hello World\n你好 世界\n123" > demo.txt
$ wc demo.txt
# 输出:  3   5   30 demo.txt
#        │   │   └── 字节数 (30, UTF-8 下中文每个占 3 字节)
#        │   └────── 单词数 (5, 以空格/换行分隔：Hello、World、你好、世界、123)
#        └────────── 行数 (3)
wc -l demo.txt                    # 行数 (lines)：3
wc -w demo.txt                    # 单词数 (words)：5
wc -c demo.txt                    # 字节数 (bytes)：30
wc -m demo.txt                    # 字符数 (chars)：22（中文按 1 字符计，区别于 -c）
wc -L demo.txt                    # 最长行长度：11 ("Hello World")
# 示例
find . | wc                       # 从管道输入统计
cat file | wc -w                  # 统计单词数
grep "error" log | wc -l          # 统计匹配行数
```
##  搜索查找命令
### `PATH` 环境变量
> **作用：** 指定系统查找命令的目录，使用户无需输入命令完整路径即可执行程序。
### `env` / `printenv` / `export`
- **`env`**：显示当前环境变量，或在修改后的环境中运行命令。
- **`printenv`**：只打印环境变量的值。
- **`export`**：设置/修改环境变量，并导出到子进程。
```shell
env                               # 查看所有环境变量
printenv PATH                     # 查看 PATH 变量
export PATH=$PATH:/usr/local/bin  # 临时添加目录到 PATH
```
### `echo`：输出给定参数
```shell
echo $PATH                        # 输出环境变量
echo $PATH | tr ':' '\n'          # 输出环境变量（按行打印）
echo "Hello World"                # 输出文本
echo -n "Hello World"             # 输出文本（不换行）
echo "Hello World" > file.txt     # 覆盖写入文件
echo "Hello World" >> file.txt    # 追加写入文件
echo -e "Column1\tColumn2"        # 启用反斜杠转义的解释（特殊字符）
echo $?                           # 输出上一条指令退出状态码（0 表示成功）
```
### `which`：查找命令路径
```shell
which java                        # $PATH 中查找第一个匹配命令路径
which [-a|--all] java             # 显示所有匹配路径
```
### `whereis`：定位命令、源码、man
```shell
# 示例：查找 ssh 的相关路径
$ whereis ssh
ssh: /usr/bin/ssh /etc/ssh /usr/share/man/man1/ssh.1.gz
#   │             │        └── man 手册路径
#   │             └── 配置文件目录
#   └── 可执行文件路径

# 常用参数（-b/-s/m）
whereis ls                        # 查找二进制/源码/man
whereis -b ls                     # 仅二进制文件（binary）
whereis -s ls                     # 仅源代码（source）
whereis -m ls                     # 仅手册页（manual）
whereis -s gcc -m git             # 同时查找 gcc 的源码和 git 的 man 手册
# 特殊用法（-u：缺失/异常）
whereis -u *                      # 列出缺少二进制/源码/man 的命令
whereis -u -b *                   # 缺少二进制
whereis -u -s *                   # 缺少源码
whereis -u -m *                   # 缺少man手册，或数量 ≠ 1（>1 或 0）
# 限定搜索路径（-B/-S/-M）
whereis -b -B /usr/bin/ -f gcc    # 仅在 /usr/bin 查找二进制
whereis -S /usr/src -f ls         # 仅在 /usr/src 查找源码
whereis -M /usr/share/man -f ls   # 仅在 /usr/share/man 查找手册
```
### `locate`：快速查找文件
```shell
sudo apt install plocate          # 安装
sudo updatedb                     # 更新数据库
# 常用选项
locate "关键字"                    # 在数据库中查找关键字
locate '*/filename'               # 精确查找文件名为 filename 的文件
locate -i readme                  # 忽略大小写查找
locate -c passwd                  # 只统计匹配结果数量
locate -n 5 passwd                # 仅显示前 5 条结果
locate -r 'passwd$'               # 使用正则匹配，以 passwd 结尾的文件
```
### `find`：实时搜索文件
* `find [路径] [条件] [操作]`
```shell
# 按文件名 / 路径
find root_path -name '*.ext'                  # 按扩展名查找
find root_path -iname '*pattern*'             # 忽略大小写
find root_path -path '*/path/*/*.ext'         # 匹配路径模式
find root_path -not -path '*/site-packages/*' # 排除指定路径
find root_path -path '*/path/*/*.ext' -or -name '*pattern*' # 匹配路径或文件名模式
find root_path -name '*.py' -not -path '*/site-packages/*'  # 查找文件，排除特定路径
# 按类型
find root_path -type f                        # 普通文件
find root_path -type d -iname '*lib*'         # 目录（忽略大小写）
find root_path -type l                        # 符号链接
find root_path -size +500k -size -10M         # 500KB~10MB
find root_path -maxdepth 1 -size +1G          # >1GB文件（递归深度1）
# 按时间（mtime=修改时间，atime=访问时间，ctime=属性变化时间）
find root_path -mtime -1                      # 最近1 天内修改过（24 小时以内）
find root_path -mtime +10                     # 10 天以前修改过（大于 10×24 小时）
find root_path -mtime 1                       # 正好1 天前修改过（24~48 小时之间）
find root_path -mmin -10                      # 最近10 分钟内修改过
find root_path -atime -7                      # 最近7 天内访问过
find root_path -ctime -3                      # 最近3 天内属性修改（权限/所有者/链接）
find root_path -daystart -mtime 0             # 从零点起算，今日修改过
# 常用操作
find root_path -name '*.ext' -exec wc -l {} \;# 对每个匹配文件执行 wc -l
find root_path -daystart -mtime -1 -exec tar -cvf archive.tar {} \+                                                           # 打包今日修改文件
find root_path -type f -empty -delete -print  # 删除空文件并打印路径
find root_path -type d -empty -delete -print  # 删除空目录并打印路径
```