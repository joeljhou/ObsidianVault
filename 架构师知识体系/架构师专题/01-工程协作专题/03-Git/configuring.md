---
title: Git 配置
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
# Git 配置
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