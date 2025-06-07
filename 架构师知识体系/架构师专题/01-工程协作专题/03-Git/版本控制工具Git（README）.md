---
title: 版本控制工具Git
shortTitle: 
description: 
icon: 
cover: 
author: 流浪码客
isOriginal: true
sticky: false
star: false
date: 2025-04-21
category: 工程协作
tags:
  - Git
---
# 版本控制工具Git（README）
[Git官网](https://git-scm.com/)
[Pro Git（中文版）在线](https://www.progit.cn/)
[图解 Git](https://marklodato.github.io/visual-git-guide/index-zh-cn.html)
[IDEA中高效使用Git](https://www.jetbrains.com/help/idea/using-git-integration.html)
## 概述及历史
1. **什么是 Git？**
	* **Git** 是一个<u>分布式</u>版本控制系统，用于跟踪文件的更改，特别是源代码文件的更改。
	* 它由 **Linus Torvalds** 于 2005 年开发，最初是为了支持 **Linux 内核开发**。
2. **什么是“版本控制”？**
	* 版本控制是一种管理和跟踪文件变更的系统
	* 允许多个用户协作开发并记录文件历史，便于回滚、合并和解决冲突。
3. **版本控制系统（VCS）的发展阶段**
	1. **本地版本控制系统 (Local Version Control System)**：
		只在本地计算机上管理文件版本，通常通过简单的目录或文件副本来保存文件的不同版本。例如： `RCS`。
	2. **集中化版本控制系统 (Centralized Version Control System, CVCS)**：
		所有版本数据存储在中央服务器上，用户通过客户端访问和提交文件的变更。缺点是如果中央服务器出现问题，可能会导致无法访问历史版本。例如：`CVS`、`Subversion (SVN)`、`Perforce`。
	3. **分布式版本控制系统 (Distributed Version Control System, DVCS)**：
		每个用户都有完整的版本历史库，允许脱机工作，并且可以将本地修改推送到中央仓库或其他用户的仓库。常见的有`Git`、`Mercurial`。
4. **Git 发展简史**
	1. **1991-2002年**：Linux 内核开发团队花费大量时间在提交补丁和保存归档上，管理工作繁琐。
	2. **2002年**：Linux 内核团队使用 **BitKeeper** 版本控制系统，但因商业合作结束，失去免费使用 BitKeeper 的权限。
	3. **2005年**：Linus Torvalds 不满意 BitKeeper，决定开发 **Git**，并设定了高效、分布式、支持非线性开发等目标。
	4. **2005年4月**：Linus 发布了 Git 的第一个版本，初步实现了分布式版本控制的功能。
	5.  **2005年后期**：Git 逐步被优化，加入了更强大的功能，如分支管理、合并、标签、历史查询等。
	6. **2008年**：Git 被 **GitHub** 所推广，成为开源项目的首选版本控制系统，极大地推动了其普及。
	7. **2010年后**：Git 成为全球开发者的主流工具，广泛应用于开源项目和企业开发。
5. **Git 核心理念**
	1. **直接记录快照，而非差异比较**：
		* Git 通过保存文件的完整快照来记录版本，而不是保存文件间的差异。
		* Git 对待数据更像是一个**快照流**。
	2. **近乎所有操作都是本地执行**：
		* Git 在本地执行几乎所有操作，不需要连接到远程仓库。
		* 赋予了离线工作的能力。
	3. **Git 保证完整性**：
		* Git 使用 SHA-1 哈希算法确保数据的完整性，保证每个版本的唯一性。
	4. **Git 一般只添加数据**：
		* Git 在每次提交时通常只增加新的数据，而不覆盖已有数据，确保数据历史的连续性。
6. **Git 的三种文件状态 与 三个工作区域****：
	*  文件有三种状态：**已提交（committed）**、**已修改（modified）**、**已暂存（staged）**。
	* 由三种状态引入了 Git 项目的**三个工作区域**：
		* **Git 仓库（Repository）**：
			* 存储提交的版本和历史记录，通常位于 `.git` 目录中。
			* 文件在提交（`git commit`）时从暂存区转移到仓库，成为项目历史的一部分。
		* **工作目录（Working Directory）**：
			* 你实际操作的文件目录，包含当前的文件状态，可以进行修改。
			* 文件的变化会使得文件处于 **已修改（Modified）** 状态。
		* **暂存区域（Staging Area 或 Index）**：
			* 暂存区域是一个临时区域，保存将要提交到 Git 仓库的文件修改。
			* 使用 `git add` 命令将文件的修改添加到暂存区域。
			* 在执行 `git commit` 命令后，暂存区域的内容会被提交到 Git 仓库。
##  Git 安装
###  Windows 安装
**官网版本安装**
* 访问 [Git 官网下载页面](https://git-scm.com/downloads/win)，选择你操作系统的版本，通常是 `64-bit Git for Windows Setup`。
**Git for Windows**
* 也叫做`msysGit`，和 Git 是分别独立的项目；
* 更多信息请访问 https://gitforwindows.org/ 。
**GitHub Desktop**
* 通过安装GitHub Desktop程序，轻松体验 Git。
* 官网号称终极GUI Git客户端，用于简化并增强你的本地代码管理流程。
* 网址为 https://github.com/apps/desktop ，支持Mac版。
### Linux 安装
**Debian/Ubuntu**
```shell
sudo apt-get install git
```
**Fedora**
```shell
sudo yum install git
```
要了解更多选择，Git 官方网站上有在各种 Unix 风格的系统上安装步骤，网址为 http://gitscm.com/download/linux。
### Mac 安装
**Homebrew**
* Git官网MacOS推荐使用Homebrew安装
```shell
brew install git
```
**Xcode Command Line Tools**
* 如果你的系统没有安装过 **Xcode Command Line Tools**，在安装 **Homebrew** 过程中，会自动提示你安装 Xcode Command Line Tools。
* 这个工具包包含了许多开发者工具，**Git** 是其中的一部分。
* 要了解更多安装选项，请访问 [Git 官网 Mac 安装页面](https://git-scm.com/downloads/mac)。
## Git 配置
Git 配置有三个层级，优先级**从低到高**依次为：

| 配置层级 | 配置文件路径                                  | 作用范围         | 配置命令                  |
| ---- | --------------------------------------- | ------------ | --------------------- |
| 系统级  | `/etc/gitconfig`                        | 系统上所有用户和所有仓库 | `git config --system` |
| 用户级  | `~/.gitconfig` 或 `~/.config/git/config` | 当前用户所有仓库     | `git config --global` |
| 仓库级  | `.git/config`                           | 当前仓库         | `git config --local`  |
1. **配置用户信息（必做）**
	* 安装完 Git 后，第一步是配置你的**用户名称**与**邮件地址**。
	* `--global` 表示**用户级**的全局配置，仅需执行一次；
	```shell
	git config --global user.name "用户名"
	git config --global user.email "邮箱"
	``` 
2. **配置文本编辑器**
	```shell
	# 将 `emacs` 设置为 Git 的默认文本编辑器
	git config --global core.editor emacs
	# 将 `VS Code` 设置为 Git 的默认文本编辑器（推荐）
	git config --global core.editor "code --wait"
	# 将 `Sublime Text` 设置为 Git 的默认文本编辑器
	git config --global core.editor "subl -n -w"
	```
3. **检查当前Git配置信息**
	* `git config --list` 命令会列出当前 Git 配置的所有项，包括全局和本地配置。
	```shell
	$ git config --list
	credential.helper=osxkeychain     # 使用 macOS Keychain 管理 Git 凭证
	init.defaultbranch=main           # 初始化 Git 仓库时，默认主分支为 'main'
	user.name=joeljhou                # Git 提交时使用的用户名
	user.email=joeljhou336@gmail.com  # Git 提交时使用的邮箱地址
	core.ignorecase=false             # 文件名区分大小写，默认值 false
	```
4. **获取 Git 命令帮助**
	* Git 提供三种方式查看命令帮助：
	```shell
	# 以 commit 命令为例
	$ git help commit
	$ git commit --help
	$ man git-commit
	```
## Git 基础
### `git config`:  配置
用于设置 Git 的配置变量，如用户名、邮箱、编辑器等
```shell
git config --global user.name "用户名"
git config --global user.email "邮箱"
```
### `git help`: 帮助文档
用于显示 Git 命令的帮助文档
```shell
git help <subcommand>
```
> 推荐使用 `tldr` 查看帮助文档
### `git init`：初始化仓库
用于在现有目录中初始化一个新的 Git 仓库
```shell
$ git init
```
### `git clone`：克隆远程仓库
用于从远程仓库创建一个 Git 仓库的拷贝
```shell
git clone <repository-url>
```
### `git add`：跟踪文件
用于将文件添加到暂存区，准备进行提交
```shell
# 添加文件/夹，多个使用“空格”隔开
git add <file>...
# 添加所有文件（包括新建、修改、删除）
git add .
```
注意：运行了`git add`之后又作了修订的文件，需要重新运行`git add` 把最新版本重新暂存起来。
### `.gitignore`：忽略文件
 格式规范如下：
1. **空行和以 `#` 开头的行**：表示注释，会被 Git 忽略。
2. **使用 glob 模式匹配文件路径**，类似简化版的正则表达式：
	* `*`：匹配任意数量的任意字符
	*  `?`：匹配单个任意字符
	* `[abc]`：匹配方括号中列出的任意一个字符
	* `[0-9]`：匹配指定范围内的字符（0~9）
	*  `**`：匹配任意中间路径，比如 `a/**/z` 可匹配 `a/z`、`a/b/z`、`a/b/c/z` 等
3. **特殊前缀符号说明**
	*  `/` 开头：表示从仓库根目录开始匹配
	* `/` 结尾：表示匹配目录
	* `!` 开头：表示“取反”，即要忽略指定模式以外的文件或目录
实际示例：
```shell
# 忽略所有 .a 文件
*.a
# 但要保留 lib.a（即使上面忽略了 .a 文件）
!lib.a
# 只忽略仓库根目录下的 TODO 文件
/TODO
# 忽略 build/ 目录及其中所有内容
build/
# 忽略 doc 目录下的 notes.txt 文件，但不忽略其他子目录下的 txt 文件
doc/*.txt
# 忽略 doc 目录下所有子目录中的 pdf 文件
doc/**/*.pdf
```
### `git diff`：比较差异（工具）
比较 Git 各个区域（工作区、暂存区、提交记录）等之间的差异。

| 比较对象            | 命令                             | 用途说明                 |
| --------------- | ------------------------------ | -------------------- |
| 工作区 vs 暂存区      | `git diff`                     | 查看未暂存的修改（最常用）        |
| 工作区+暂存区 vs HEAD | `git diff HEAD`                | 查看所有未提交的修改（含已暂存和未暂存） |
| 暂存区 vs 最新提交     | `git diff --staged`            | 查看已暂存但未提交的修改         |
| 统计变更信息          | `git diff --stat`              | 只看变更数量，不看具体内容        |
| 总结结构变化          | `git diff --summary`           | 看重命名、新建、删除、权限等       |
| 任意时间点 vs HEAD   | `git diff 'HEAD@{n days ago}'` | 查看过去的变更（使用频率中等）      |
| 比较两个分支间文件       | `git diff 分支1..分支2 -- 路径/文件`   | 比较两个版本中同一文件的差异       |
| 比较跨分支的两个文件      | `git diff 分支:路径/文件 路径/本地文件`    | 对比任意两个文件             |
图形工具比较差异：`git difftool`
```shell
# 使用 vscode 作为 git difftool
git config --global diff.tool vscode
git config --global difftool.vscode.cmd "code --wait --diff $LOCAL $REMOTE"
```
### `git commit`：提交更新
```shell
# 提交当前已暂存（staged）的修改。
git commit -m "提交暂存区"
# 将跟踪文件的修改，添加到暂存区并提交
git commit -a -m "跳过使用暂存区域"
# 将当前暂存的更改追加到最近一次提交，并更新其哈希值
git commit --amend -m "追加提交"
```
### `git rm`：移除文件
```shell
# 删除文件
$ rm <file>
# 删除文件，并标记为待提交删除
$ git rm <file>
# 递归删除文件
$ git rm -r	<file>
# 仅取消追踪，文件保留在本地
$ git rm --cached <file>
```
**支持 Glob 模式**：允许使用通配符来匹配文件。
```shell
# 递归删除整个目录及其子目录中的所有文件。慎用！
git rm -r *
# 删除 `log/` 目录下的 `.log` 文件
# 如果没有反斜杠，Shell 会先将 `*.log` 展开成实际的文件列表
git rm log/\*.log
# 删除所有以 `~` 结尾的文件
# 反斜杠作用，避免Shell 将 `*~` 展开成所有符合该模式的文件，再传递给 `git rm`。
git rm \*~
# 递归删除 `docs` 目录下的 `.md` 文件
git rm -r 'docs/*.md'
```
### `git mv`：移动文件
如果在 Git 中重命名了某个文件，仓库中存储的元数据并不会体现出这是一次改名操作。不过 Git 非常聪明，它会推断出究竟发生了什么。
```shell
$ git mv README.md README
$ git status             
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	renamed:    README.md -> README
```
相当于运行了下面三条命令：
```shell
mv README.md README
git rm README.md
git add README
```
### `git status`：文件状态
Git 工作目录中的文件状态主要分为两类：
1. **已跟踪文件**：即被版本控制的文件
	* 工作一段时间后，这些文件可能会处于以下几种状态：
		* **未修改**（`Unmodified`）：文件内容自上次提交以来没有变化，保留在当前版本中。
		* **已修改**（`Modified`）：文件的内容有变化，但这些修改还未被加入暂存区。
		* **已暂存**（`Staged`）：文件修改后已经被添加到暂存区，准备提交这些变动。
2. **未跟踪文件**（`Untracked`）：未被版本控制，或忽略的文件
	* 🉑通过`git add <filename>`将未跟踪文件添加到暂存区，纳入版本控制。
文件的状态变化周期示意图：
![文件的状态变化周期](http://img.geekyspace.cn/pictures/2025/20250502004429356.png)

✅ **`git status`（默认详尽输出）**
用于查看工作目录和暂存区的文件状态：
1. **✔️干净的工作目录：`nothing to commit, working tree clean`**
	* 没有任何改动，工作区和暂存区一致
2. **📦已暂存的更改：`Changes to be committed`**
	* `new file:` 新增文件并已暂存
	* `modified:` 文件内容有修改并已暂存
	* `deleted:` 文件已被删除并暂存
3. **📝有文件被修改但未暂存：`Changes not staged for commit`**
	- `modified:` 文件被修改但未暂存
	* `deleted:` 文件被删除但未暂存
4. **🆕未追踪文件：`Untracked files`**
	* 文件是新添加的，但 Git 尚未跟踪（未使用 `git add`）
✅ **状态简览（-s）**
`git status -s`（或 `--short`）提供更**紧凑**的状态摘要，适合快速查看当前变更。
**示例：**
```shell
$ git status -s
 M README
MM Rakefile
A  lib/git.rb
M  lib/simplegit.rb
?? LICENSE.txt
```
**速记：**
* 左边代表已暂存，右边代表已修改；
*  `M`是修改，`A`是添加，`??`是未追踪。
### `git log`：查看提交历史     
```shell
$ git log
commit 🔐SHA-1 (HEAD -> master)
Author: 🧑‍💻作者 <📮邮箱>
Date:   2025-05-20 00:00:00 +0800

    ✨ 提交说明（如：优化首页加载速度）
```
#### 常用选项
![[Pro Git 中文版 第2版 2.1.1.pdf#page=41&rect=46,253,567,486|Pro Git 中文版 第2版 2.1.1, p.31]]
✅ **`-p`：显示每次提交的差异（diff）**
```shell
$ git log -p

commit a1b2c3d4
Author: Alice <alice@gmail.com>
Date:   Fri May 9 17:22:17 2025 +0800

    修改欢迎语

diff --git a/main.js b/main.js
--- a/main.js
+++ b/main.js
@@ -1,4 +1,4 @@
-console.log("Hello, world!");
+console.log("Welcome to my site!");
```
✅ **`--stat`：显示每次提交涉及的文件及统计信息**
```shell
$ git log --stat

commit a1b2c3d4
Author: Alice <alice@example.com>
Date:   Fri May 9 17:22:17 2025 +0800

    修改欢迎语

 main.js | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
```
✅ **`--shortstat`：仅显示修改统计汇总**
```shell
$ git log --shortstat

commit a1b2c3d4
Author: Alice <alice@example.com>
Date:   Fri May 9 17:22:17 2025 +0800

    修改欢迎语

 1 file changed, 1 insertion(+), 1 deletion(-)
```
✅ **`--name-only`：仅显示受影响文件**
```shell
$ git log --name-only

Acommit a1b2c3d4
Author: Alice <alice@example.com>
Date:   Fri May 9 17:22:17 2025 +0800

    修改欢迎语

main.js
```
✅ **`--abbrev-commit`：使用简短提交 ID**
```shell
$ git log --abbrev-commit

commit a1b2c3d
Author: Alice <alice@example.com>
Date:   Fri May 9 17:22:17 2025 +0800

    修改欢迎语
```
✅ **`--relative-date`：以相对时间显示日期**
```shell
$ git log --relative-date

commit a1b2c3d4
Author: Alice <alice@example.com>
Date:   2 hours ago

    修改欢迎语
```
✅ **`--graph`：图形化查看分支历史**
* 可以与`oneline`或`format`结合使用
```shell
$ git log --graph --oneline

* a1b2c3d4 修改欢迎语
* | f1e2d3c4 修复按钮样式
| * 2b3c4d5e 添加用户登录功能
|/
* 3a4b5c6d 初始化项目
```
✅ **`--decorate`：显示分支名和标签**
```shell
$ git log --oneline --decorate --graph --all
```
✅ **`--pretty`：自定义格式输出**
```shell
$ git log --pretty=oneline
$ git log --pretty=format:"%h - %an, %ad : %s" --date=local

* a1b2c3d4 - Alice, 2025年5月9日 17:22 : 修改欢迎语
* | f1e2d3c4 - Bob, 2025年5月8日 14:45 : 修复按钮样式
| * 2b3c4d5e - Carol, 2025年5月7日 12:30 : 添加用户登录功能
|/
* 3a4b5c6d - Alice, 2025年5月5日 09:12 : 初始化项目
```
![[Pro Git 中文版 第2版 2.1.1.pdf#page=40&rect=44,285,570,629|Pro Git 中文版 第2版 2.1.1, p.31]]
#### 限制输出长度
![[Pro Git 中文版 第2版 2.1.1.pdf#page=42&rect=44,279,562,460|Pro Git 中文版 第2版 2.1.1, p.38]]
✅ **限制输出条数**
```shell
# 查看最近 5 次提交
git log -2
```
✅ **限制输出时间**
* 🕒 时间格式参考：
	* 绝对时间：如 `2025-05-10`、`2025-05-10 14:00`
	* 相对时间：如 `2 years 1 day 3 minutes ago`、`last monday`、`1 week ago`
```shell
# 查看指定时间之后的提交（since/after 等价）
git log --since=2.weeks
git log --after="2025-05-01"

# 查看指定时间之前的提交（until/before 等价）
git log --until=2.weeks
git log --before="2025-05-01"
```
✅ **按作者/提交者筛选**
```shell
# 查看 Alice 提交的记录
git log --author="Alice"

# 查看由 Carol 提交（即由她运行 git commit）的记录
git log --committer="Carol"
```
✅ **按提交说明模糊筛选**
```shell
# 显示所有包含“欢迎”的提交信息
git log --grep="欢迎"
```
✅ **多条件组合筛选：使用 `--all-match`（AND 逻辑）**
```shell
# 提交者是 Alice 且说明中包含“欢迎”
git log --author="Alice" --grep="欢迎" --all-match

# 同时包含多个关键词（必须都命中）
git log --grep="欢迎" --grep="登录" --all-match
```
✅ **精确追踪代码内容变动：使用 `-S`**
```shell
# 查找添加或移除了字符串 "Welcome" 的提交
git log -S"Welcome"

# 结合 -p 查看修改细节
git log -S"Welcome" -p
```
### `git remote`：管理远程仓库
✅ **列出远程仓库简写名**
```shell
$ git remote
origin
```
* `origin` 是默认的远程仓库名称
✅ **查看远程仓库地址（-v）**
```shell
# 查看详细的 URL 信息
$ git remote -v
gk	    https://github.com/joeljhou/geekyspace.git (fetch)
gk	    https://github.com/joeljhou/geekyspace.git (push)
origin	git@github.com:joeljhou/geekyspace.git (fetch)
origin	git@github.com:joeljhou/geekyspace.git (push)
```
✅ **查看远程仓库详情（show）**
* 语法：`git remote show [remotename]`
* 显示远程仓库的完整配置信息
	* `tracked`: 本地已有对应的远程分支引用；
	* `new`: 是远程新增的分支，尚未在本地存在；
	* `stale`: 远程已删除，本地仍保留，可用 `git remote prune` 清除。
```shell
$ git remote show origin
* remote origin
  Fetch URL: git@github.com:joeljhou/geekyspace.git
  Push  URL: git@github.com:joeljhou/geekyspace.git
  HEAD branch: master
  Remote branches:
    master   tracked
    dev      new (next fetch will store in remotes/origin)
    old      stale (use 'git remote prune' to remove)
  Local branch configured for 'git pull':
    master merges with remote master
  Local ref configured for 'git push':
    master pushes to master (up to date)
```
✅ **添加远程仓库（add）**
* 语法：`git remote add <shortname> <url> `
```shell
git remote add gk https://github.com/joeljhou/geekyspace.git
```
添加后可在后续命令中使用简写名 `gk` 来代替完整 URL。
✅ **重命名远程仓库（rename）**
```shell
git remote rename gk gkspace
```
✅ **移除远程仓库（rm）**
```shell
git remote rm gkspace
```
✅ **列出远程仓库引用（ls-remote）**
```shell
# 默认列出当前项目远程仓库（origin）的所有引用（HEAD、分支、标签等）
$ git ls-remote
e1d89f88460e2cde9ed45f738dfc5b12fa742fdc	HEAD
e1d89f88460e2cde9ed45f738dfc5b12fa742fdc	refs/heads/master
8a7fe3c52cbf2aeb7e05830a1b2169cf4a0cfa41	refs/tags/v1.0.0
# 指定远程仓库名（如 origin）列出其所有引用
$ git ls-remote origin
# 仅显示远程分支（refs/heads/）
$ git ls-remote --heads
# 仅显示远程标签（refs/tags/）
$ git ls-remote --tags
```
### `git restore`：恢复文件
```shell
# ===== git restore：恢复文件 =====
$ git restore file                      # 恢复指定文件到暂存区（撤销工作区的修改）
$ git restore :/                        # 撤销工作区所有修改

# 分支/提交中检出文件（别名👉 ：git extract）
$ git restore --source <commit-or-branch> file
$ git extract HEAD~2 src/main.java      # 两次提交前的版本
$ git extract feature/login config.yml  # 指定分支

# 取消暂存文件（别名👉 ：git unstage）
$ git restore --staged file
$ git unstage :/                        # 取消暂存所有文件
$ git unstage app.py                    # 取消暂存app.py文件

# 丢弃所有改动
$ git restore --staged --worktree :/    # 恢复到上一次提交
$ git restore -SW :/                    # 简写

# 交互式选择恢复某些内容（精细控制）
$ git restore -p
```
### `git fetch`：抓取
语法：`git fetch [remotename]`
```shell
$ git fetch origin    # 从指定远程仓库抓取最新代码，但不自动合并
$ git fetch           # 简写（设置上游后）
$ git fetch --all     # 抓取所有远程仓库的更新
$ git fetch --prune   # 清理已被远程删除的本地引用
```
执行命令后，你将会拥有远程仓库中所有分支的引用，**需手动合并**。
### `git pull`：拉取
语法：`git pull [remotename] [branch]`
```shell
A---B---C  (origin/main)
     \
      D---E  (本地提交)
# 拉取并合并（fetch + merge）
$ git pull origin main
$ git pull
A---B---C---M  (合并提交 M)
     \     /
      D---E
# 拉取并变基（fetch + rebase）
$ git pull --rebase
A---B---C---D'---E'  (本地提交 D 和 E 被“重放”为 D' 和 E'，历史线性)
```

### `git push`：推送
语法：`git push [remotename] [branch]`
```shell
$ git push origin serverfix
# 有些工作被简化了，相当于执行了
$ git push origin refs/heads/serverfix:refs/heads/serverfix
$ git push origin serverfix:serverfix
# 将本地的 serverfix 分支推送到远程仓库上的 awesomebranch 分支
$ git push origin serverfix:awesomebranch

# 强制推送覆盖
$ git push --force
# 先检查远程状态是否变更，条件允许时覆盖
$ git push --force-with-lease
```
 ⚠️ 注意事项：
 * 你必须对目标远程仓库拥有**写入权限**；
 * 如果这是该分支**首次推送**，Git 会在远程创建对应分支；
 * 如果远程已有更新，而你本地没有同步，推送会被**拒绝**，必须先拉取并合并远程的最新更改。
✅ **删除远程分支**
* 语法：`git push <remotename> --delete <branch>`
```shell
$ git push origin --delete feature/xxx
# 等效的老写法
$ git push origin :feature/xxx
```
### `git tag`：管理标签
✅ **列出标签**
* 标签默认以**字母顺序**列出，但顺序没有实际意义。
```shell
$ git tag 
v0.1
v1.3
```
✅ **模糊匹配标签名（-l）**
```shell
$ git tag -l 'v1.8.5*'
v1.8.5
v1.8.5-rc0
v1.8.5.1
```
✅ **查看标签**
* 语法：`git show <tag>`
```shell
# 显示标签指向的提交详情
$ git show v1.0
```
✅ **创建标签（-a）**
Git 支持两种类型的标签：
1. 🪶 轻量标签（`lightweight`）：临时标记，不含元信息，仅指向某次提交。
	* 语法：`git tag <tag>`
2. 📝 **附注标签**（`annotated`）：独立对象，含作者、时间、说明，可签名。**推荐**用于发布版本。
	* 语法：`git tag -a <tag> -m "message"` 
* 🕰️ 历史提交打标签
	* 语法：`git tag -a <tag> <commit-id> -m "message"`
```shell
# 1.轻量标签
$ git tag v1.0-lw
# 2.附注标签
$ git tag -a v1.0 -m "发布版本 v1.0"
# 3.历史提交打标签
$ git log --pretty=oneline. # 查找历史提交 ID
$ git tag -a v0.9 abc1234 -m "发布 v0.9"
```
✅ **共享（推送）标签**
* `git push` 不会自动推送标签到远程仓库。你需要手动推送标签。
```shell
# 推送单个/多个标签
git push origin [tag]...
# 推送所有本地标签
git push origin --tags
```
✅ **删除标签（-d）**
* 本地标签语法：`git tag -d <tag>` 
*  远程标签语法：`git push <remotename> :refs/tags/<tag>`
	* `:` 左侧为空，表示“推送空引用”
	* `refs/tags/<tag>` 是远程标签的完整引用路径，可简写为`<tag>`。
```shell
# 删除本地标签
$ git tag -d v1.0
Deleted tag 'v1.0' (was abc1234)

# 删除远程标签
$ git push origin :v1.0
To origin
 - [deleted]         v1.0
```
✅ **检出标签**
* 即在特定的标签上创建一个新分支
* 语法：`git checkout -b <new-branch> <tag>`
* 新版本推荐：`git switch -c <new-branch> <tag>`
```shell
# 基于 v2.0.0 标签创建一个可编辑的新分支"version2"
$ git checkout -b version2 v2.0.0
Switched to a new branch 'version2'
```
## 命令别名设置
```shell
# 简写别名设置
$ git config --global alias.co checkout
$ git config --global alias.br branch
$ git config --global alias.ci commit
$ git config --global alias.st status
# 自定义实用别名
$ git config --global alias.slog=log --oneline --decorate --graph --all
$ git config --global alias.unstage 'restore --staged'  # 取消暂存文件
$ git config --global alias.extract 'restore --source'  # 分支/提交中检出文件
$ git config --global alias.last 'log -1 HEAD'          # 查看最后一次提交
```
**运行外部命令**
如果你想让别名执行外部命令而不是 Git 子命令，可在别名前加 `!`：
```shell
# 用法：git visual 启动 gitk 图形界面
git config --global alias.visual '!gitk'
```
## Git 分支
> Git 的分支系统极其轻量，这是它的“**必杀技**”。分支的**创建**和**合并**是 Git 工作流的核心。

一个项目提交时，Git 会将项目结构保存为如下对象：
```shell
project/
├── README.md  → blob
├── src/
│   └── main.java → blob
└── LICENSE → blob
```
- `blob`：保存文件内容。
- `tree`：保存目录结构和文件引用。
- `commit`：保存一次提交，包含指向 `tree` 的指针和提交信息。
- `branch`：一个指向某个 `commit` 的可变指针。
Git 分支本质上是一个指向最新提交的**可变指针**，每次提交都会自动向前移动。
![[Pro Git 中文版 第2版 2.1.1.pdf#page=58&rect=38,481,574,742|Pro Git 中文版 第2版 2.1.1, p.51]]
### `git branch`：分支管理
✅ **列出分支**
* 语法：`git branch`
```shell
$ git branch             # 列出本地分支，当前分支带 *
$ git branch -r          # 列出remote分支
$ git branch -vv         # 详细输出，本地分支和远程分支的同步状态及最后提交情况
$ git branch --merged    # 查看已合并的分支（可考虑删除）
$ git branch --no-merged # 查看未合并的分支（可考虑合并）
```
🔁 **设置或修改上游分支（远程追踪）**
```shell
$ git branch --set-upstream-to=origin/dev local-dev
# 简写
$ git branch -u origin/dev local-dev
```
设置后可以使用 `git pull` / `git push` 直接与远程交互，或用 `@{u}` 引用上游分支。
✅ **新建分支**
* 语法：`git branch <branch>`
```shell
$ git branch hotfix      # 创建名为 hotfix 的新分支（不会切换）
```
✅ **新建并切换分支（-b）**
* 语法：`git branch -b <branch>`
```shell
$ git checkout -b iss53  # 等价于`git branch` + `git checkout`
Switched to a new branch "iss53"
# 看到`HEAD`指向了当前分支`iss53`
$ git slog  # 等价于`git log --oneline --decorate --graph --all`
* f30ab (HEAD -> iss53) 修复数据处理逻辑
* 34ac2 (main) 增加用户登录功能
| * ab28c (hotfix) 修复线上bug [issue 53]
|/
* 98ca9 第一次代码提交
* ed596 初始化工程
```
✅ **删除分支（-d/-D）**
* 语法：`git branch -d <branch>`
```shell
# 安全删除已合并的分支
$ git branch -d hotfix
# 强制删除（即使未合并）
$ git branch - testing
```
### `git checkout`：瑞士军刀
git checkout 就像一个多功能瑞士军刀，功能强大但不够专一。
涵盖了 **分支切换/创建、文件恢复/检出、远程分支跟踪** 等多种功能。
> ⚠️ 从 Git 2.23 版本起，官方拆分命令：
> 	`git switch` 专注于分支切换
> 	`git restore` 专注于文件恢复  
> 以提供更清晰的语义和更安全的操作。

❌ **分支切换/创建**
```shell
# 切换分支
$ git checkout <branch> 
# 创建并切换到新分支
$ git checkout -b <branch>
```
❌ **文件恢复**
```shell
$ git checkout -- README.md    # 恢复指定文件到暂存区（撤销工作区的修改）
$ git checkout -- .            # 撤销工作区所有修改

# 分支/提交中检出文件
$ git checkout <commit-or-branch> -- <file>
$ git checkout HEAD~1 -- src/main.py
$ git checkout feature/login -- config.yaml
```
🌐 **远程跟踪分支（--track）**
* 语法：`git checkout -b [branch] [remotename]/[branch]`
```shell
# 基于远程分支创建本地分支，自动跟踪
$ git checkout -b dev origin/dev
# 等价简写
$ git checkout --track origin/dev

# 本地分支名与远程分支名不同，需手动指定跟踪上游
$ git checkout -b local-dev origin/dev
$ git branch --set-upstream-to=origin/dev local-dev
```
### `git switch`：分支切换
语法：`git switch <branch>`
```shell
# 切换分支
git checkout testing
# 创建并切换到新分支
git switch -c <branch>
# 基于远程分支创建本地分支，自动跟踪
git switch --track origin/dev
```
这样 `HEAD` 就指向 testing 分支了。
### `git merge`：合并
语法：`git merge <branch>`
合并的三种常见场景：
1. **快进合并（Fast-forward Merge）**
	* 当前分支（ `main`）没有任何新的提交，完全落后于目标分支（ `feature`）。
	* 本质是直接把指针“**快进**（fast-forward）”到目标分支。
	```shell
	# 📈 合并前
	A---B---C (feature)
         \
          main (指针快进到 C)
    
    # 📈 合并后
    A---B---C (main, feature)
	```
 2. **三方合并（Three-way Merge）**
	 * 当前分支（ `main`）和要合并分支 （`feature`）各自有新的提交，分支点之后有分叉。
	 * Git 会以**共同祖先节点**、两个分支各自末端快照为依据，**生成一个新的合并提交**。
	 ```shell
	# 📈 合并前
	      D---E (feature)
	     /
	A---B---C (main)
	
	# 📈 合并后
		  D---E (feature)
	     /     \
	A---B---C---M (main)
	```
 3. **冲突合并（Merge Conflict）**
	 * 当三方合并中两个分支**修改了同一文件的同一位置**，Git 无法自动合并，发生冲突。
	 * 合并操作会中止，需要手动介入解决冲突。
📄 冲突合并示意（index.html）：
```shell
$ git switch main
$ git merge feature
Auto-merging index.html
CONFLICT (content): Merge conflict in index.html
Automatic merge failed; fix conflicts and then commit the result.
# 查看冲突文件
$ cat index.html
<<<<<<< HEAD
<div id="footer">contact : email.support@github.com</div>
=======
<div id="footer">
 please contact us at support@github.com
</div>
>>>>>>> feature
```
手动编辑冲突文件，清除冲突标记 `<<<<<<<` `=======` `>>>>>>>`。 
### `git rebase`：变基
语法：`git rebase <branch>` 
* 把**当前所在分支**的提交“搬移”到 `<branch>` 分支的最新提交之后。
**变基的基本操作**　
```shell
# 变基前
master:     A---B---C
                 \
experiment:        D---E

# 执行命令
# 将 `experiment` 分支的提交接到 `master` 最新提交之后
$ git switch experiment
$ git rebase master

# 变基后
master:     A---B---C
                      \
experiment:             D'---E'

# 回到 master 分支，进行一次“快进合并”
$ git checkout master
$ git merge experiment

# 最终结构
master:     A---B---C---D'---E'
                             ↑
experiment:                  |
```
将 `experiment` 分支的提交移动到 `master` 的最新提交之后。
**更有趣的变基例子**
语法：`git rebase --onto <newbase/新家> <upstream/要剪谁> <branch/要搬谁>`
```shell
# 变基前
A---B---C---D           (master)
         \
          E---F---G     (server)
               \
                H---I   (client)

# 执行命令
# 将client分支上，相对于 server 的提交（H, I）移到 master 分支后面，同时丢弃 server 分支的上下文。
$ git rebase --onto master server client
# 等同于：
git checkout client
git rebase --onto master server

# 变基后
A---B---C---D---H'---I'   (master->D, client)
         \
          E---F---G       (server)


# 回到 master 分支，进行一次“快进合并”
$ git switch master
$ git merge client

# 最终结构
           ​\
          E---F---G       (server)
```
## 服务器上的 Git
### 协议
Git支持四种主要协议用于数据传输：**本地协议**、**HTTP协议**（智能/哑）、**SSH协议**和**Git协议**。
✅ **本地协议（Local protocol）**
远程版本库就是硬盘内的另一个目录。
克隆一个本地版本库，可以执行如下的命令：
```shell
# 本地路径，使用硬链接（hard links）方式加快克隆速度
$ git clone /opt/git/project.git

# file:// 认为是一个网络URL，使用HTTP一样的"哑协议”方式
$ git clone file:///opt/git/project.git
```
**👍 优点：**
* 团队已经有共享文件系统，建立版本库会十分容易。
**👎 缺点：**
* 共享文件系统比较难配置，不方便从多个位置访问。
* 通过 NFS 访问版本库要比通过 SSH 访问慢。
* 这个协议并不保护仓库避免意外的损坏。
✅  **HTTP 协议**
Git 支持两种 HTTP 协议：**哑（Dumb）HTTP** 和 **智能（Smart）HTTP**，其中智能 HTTP在 Git 1.6.6 之后成为主流。
**智能（Smart） HTTP 协议**
* 最流行的使用 Git 的方式
* 即支持像`git://`协议一样设置匿名服务，也可以像`SSH`协议一样提供传输时的授权和加密。
**哑（Dumb） HTTP 协议**
* 哑 HTTP 协议里 web 服务器仅把裸版本库当作普通文件来对待，提供文件服务。
* 仅支持只读操作（克隆、拉取），适合轻量只读发布。
**👍 优点：**（只关注智能 HTTP 协议）
* 访问只需要一个URL，终端用户操作简单
* 相比 SSH 协议，可以使用用户名/密码授权，无需设置 SSH 密钥。 
* 对非资深的使用者，HTTP 协议的可用性是主要的优势。
**👎 缺点：**
* 在 HTTP 上使用需授权的推送，管理凭证麻烦。
✅  **SSH 协议**
SSH 协议也是一个验证授权的网络协议；并且，因为其普遍性，架设和使用都很容易。
通过 SSH 协议克隆版本库：
```shell
# 指定一个 `ssh://` 的 URL：
$ git clone ssh://user@server/project.git
# scp 式的写法：
$ git clone user@server:project.git
```
**👍 优点：**
* SSH 架设相对简单，部署和管理门槛低。
* 安全性高，所有传输数据都要经过授权和加密。
* 传输效率高。
👎 缺点：
* 不支持匿名访问。
✅  **Git 协议**
Git 协议是一种专为 Git 设计的高效只读协议：
* 监听默认端口 `9418`。
* 使用时需在裸仓库根目录创建 `git-daemon-export-ok` 文件，表示允许通过 Git 协议访问。
* 不支持授权机制，因此无法通过 Git 协议推送。
**👍 优点：**
* 网络传输协议里最快的。
👎 缺点：
* 缺乏授权机制，不适合作为访问项目版本库的唯一手段。 
* 部署复杂，需要配置 `git-daemon` 守护进程，通常还需配合 `xinetd` 或类似服务管理工具。
* 需开放防火墙端口 `9418`。
### 在服务器上搭建 Git（略）
###  生成 SSH 公钥
> 关于在多种操作系统中生成 SSH 密钥的更深入教程，请参阅 GitHub 的 SSH 密钥指南 [https://help.github.com/articles/generating-ssh-keys](https://help.github.com/articles/generating-ssh-keys)。
```shell
ssh-keygen -t ed25519 -C "your_email@example.com"
```
参考文档：
* [简单配置实现多个GitHub和Gitee账号的SSH管理](https://blog.csdn.net/qq_40174960/article/details/131298464)
### 配置服务器...
### Git 守护进程...
### Smart HTTP...
### GitWeb
如果你对项目有读写权限或只读权限，你可能需要建立起一个基于网页的简易查看器。 Git 提供了一个叫做 GitWeb 的 CGI 脚本来做这项工作。
![GitWeb 的网页用户界面](http://img.geekyspace.cn/pictures/2025/20250528012638682.png)
### GitLab（极狐）
更现代，功能更全的 Git 开源服务器。
[极狐GitLab Docker部署](https://gitlab.cn/docs/jh/install/docker.html)
```shell
# 设置环境变量
export GITLAB_HOME=$HOME/docker-data-volumes/gitlab/
# 宿主机永久设置
echo 'export GITLAB_HOME=$HOME/docker-data-volumes/gitlab/' >> ~/.zshrc
source ~/.zshrc

# 创建挂载目录
mkdir -p $GITLAB_HOME/{config,logs,data}

# 运行极狐 GitLab，启用 amd64 模拟
sudo docker run --detach \
  --platform linux/amd64 \
  --hostname gitlab.local \
  --publish 80:80 --publish 8443:443 --publish 2222:22 \
  --name gitlab-jh \
  --restart unless-stopped \
  --volume $GITLAB_HOME/config:/etc/gitlab \
  --volume $GITLAB_HOME/logs:/var/log/gitlab \
  --volume $GITLAB_HOME/data:/var/opt/gitlab \
  --shm-size 1024m \
  registry.gitlab.cn/omnibus/gitlab-jh:latest
```
首次登录使用用户名`root`，通过如下方式获取密码。
```shell
# Linux安装包方式
sudo cat /etc/gitlab/initial_root_password
# Docker安装方式
sudo docker exec -it gitlab-jh grep 'Password:' /etc/gitlab/initial_root_password
```
方便学习使用的小设置：
* [修改密码](https://support.huaweicloud.com/bestpractice-ecs/zh-cn_topic_0142594569.html)
	```shell
	jh@2025!
	```
* [偏好设置-本地化中设置中文](https://jihulab.com/gitlab-cn/gitlab/-/issues/3685)
* 新增`host`文件配置：`127.0.0.1 gitlab.local`
* 直接映射端口号`80`，以便直接通过域名`gitlab.local`访问
参考文档：
* [阿里云官方文档-部署GitLab代码托管平台](https://help.aliyun.com/zh/ecs/use-cases/deploy-and-use-gitlab)
* [Gitlab管理用户、组、权限(一)](https://www.cnblogs.com/zangxueyuan/p/9222014.html)
## Github
### 账户的创建和配置
* [官网](https://github.com/)
* [注册](https://github.com/signup)
* SSH 访问
	* [简单配置实现多个GitHub和Gitee账号的SSH管理](https://blog.csdn.net/qq_40174960/article/details/131298464)
* [设置邮箱](https://github.com/settings/emails)
* [双因素身份验证-2FA](https://github.com/settings/security)
### 对项目做出贡献
* 派生（Fork）项目
