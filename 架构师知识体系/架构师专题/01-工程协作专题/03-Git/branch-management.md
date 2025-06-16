---
title: Git 分支管理
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
# Git 分支管理
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
## `git branch`：分支管理
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
## `git checkout`：瑞士军刀
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
## `git switch`：分支切换
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
## `git merge`：合并
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
## `git rebase`：变基
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

