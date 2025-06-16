---
title: Git 核心命令
shortTitle: 
description: 
icon: 
cover: 
author: 流浪码客
isOriginal: true
sticky: false
star: false
date: 2025-06-07
category: 工程协作
tags:
  - Git
---
# Git 核心命令
## `git config`:  配置
用于设置 Git 的配置变量，如用户名、邮箱、编辑器等
```shell
git config --global user.name "用户名"
git config --global user.email "邮箱"
```
## `git help`: 帮助文档
用于显示 Git 命令的帮助文档
```shell
git help <subcommand>
```
> 推荐使用 `tldr` 查看帮助文档
## `git init`：初始化仓库
用于在现有目录中初始化一个新的 Git 仓库
```shell
$ git init
```
## `git clone`：克隆远程仓库
用于从远程仓库创建一个 Git 仓库的拷贝
```shell
git clone <repository-url>
```
## `git add`：跟踪文件
用于将文件添加到暂存区，准备进行提交
```shell
# 添加文件/夹，多个使用“空格”隔开
git add <file>...
# 添加所有文件（包括新建、修改、删除）
git add .
```
注意：运行了`git add`之后又作了修订的文件，需要重新运行`git add` 把最新版本重新暂存起来。
## `.gitignore`：忽略文件
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
## `git diff`：比较差异（工具）
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
## `git commit`：提交更新
```shell
# 提交当前已暂存（staged）的修改。
git commit -m "提交暂存区"
# 将跟踪文件的修改，添加到暂存区并提交
git commit -a -m "跳过使用暂存区域"
# 将当前暂存的更改追加到最近一次提交，并更新其哈希值
git commit --amend -m "追加提交"
```
## `git rm`：移除文件
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
## `git mv`：移动文件
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
## `git status`：文件状态
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
## `git log`：查看提交历史     
```shell
$ git log
commit 🔐SHA-1 (HEAD -> master)
Author: 🧑‍💻作者 <📮邮箱>
Date:   2025-05-20 00:00:00 +0800

    ✨ 提交说明（如：优化首页加载速度）
```
### 常用选项
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
### 限制输出长度
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
## `git remote`：管理远程仓库
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
## `git restore`：恢复文件
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
## `git fetch`：抓取
语法：`git fetch [remotename]`
```shell
$ git fetch origin    # 从指定远程仓库抓取最新代码，但不自动合并
$ git fetch           # 简写（设置上游后）
$ git fetch --all     # 抓取所有远程仓库的更新
$ git fetch --prune   # 清理已被远程删除的本地引用
```
执行命令后，你将会拥有远程仓库中所有分支的引用，**需手动合并**。
## `git pull`：拉取
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
## `git push`：推送
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
## `git tag`：管理标签
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
## 🚀命令别名设置
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

