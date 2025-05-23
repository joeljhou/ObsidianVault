## 终端
### [git](https://git-scm.com/)
```shell
brew install git
which git
git --version

# 安装gitk
brew install git-gui
```
- XCode CLI 安装时会包含`git`，推荐使用 `brew` 来安装最新版本。
### [python3](https://www.python.org/)
```
brew install python3
which python3
python3 --version
```
* XCode CLI 安装时会包含`python3`，推荐使用 `pyenv` 来进行 Python 多版本管理。
### wget
```shell
brew install wget
which wget
wget --version
```
### curl
```shell
brew install curl
which curl
curl --version
```
- **macOS 自带了 `curl`**，Xcode CLI 中也包含了 `curl`。

### tree
```shell
brew install tree
which tree
tree --version
```
### JDK
```shell
sdk install java
which java
java --version
javac
```
* 推荐使用 `SDKMAN` 来进行 JDK 的多版本管理。
### [nodejs](https://nodejs.org/zh-cn)
```shell
nvm install 20
```
* [nvm](https://github.com/nvm-sh/nvm) 的维护者明确表示 `brew` 不是官方推荐的安装方式。

### pnpm


## 客户端
安装软件前先通过终端启用“任何来源”（推荐）
```shell
sudo spctl --master-disable
```
### Mos
[Mos官网](https://mos.caldis.me/)推荐使用[Homebrew](https://brew.sh/)来安装:
```shell
# 安装
$ brew install --cask mos
# 更新
$ brew update
$ brew reinstall mos
```
### IDEA
[官网](https://www.jetbrains.com/idea/)
IntelliJ IDEA 是一个强大的 IDE，广泛用于 Java 开发和其他多种编程语言。

### VS Code
[官网](https://code.visualstudio.com/)
Visual Studio Code 是一个轻量级的代码编辑器，支持多种编程语言并具有丰富的插件生态。
### SSH客户端
SecureCRT Mac
https://termius.com/

### Navicat
* 投毒： https://www.bilibili.com/video/BV1Su411X7cL

### LuLu
开源防火墙
* 官网： https://objective-see.org/products/lulu.html
* up主推荐： https://www.bilibili.com/video/BV1ES4y1Q7BD
