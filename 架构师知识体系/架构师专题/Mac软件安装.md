## 终端
### [on-my-zsh](https://ohmyz.sh/#install)
* 主题
* 插件
	* git
	* 自动补全：zsh-autosuggestions
	* 语法高亮：zsh-syntax-highlighting
### [git](https://git-scm.com/)
```shell
brew install git
which git
git --version

# 安装gitk
brew install git-gui
```
- XCode CLI 安装时会包含`git`，推荐使用 `brew` 来安装最新版本。
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
### [python3](https://www.python.org/)
```
brew install python3
which python3
python3 --version
```
* XCode CLI 安装时会包含`python3`，推荐使用 `pyenv` 来进行 Python 多版本管理。
### [nodejs](https://nodejs.org/zh-cn)
```shell
nvm install 20
```
* [nvm](https://github.com/nvm-sh/nvm) 的维护者明确表示 `brew` 不是官方推荐的安装方式。
### [tldr](https://tldr.inbrowser.app/)
`tldr` 的含义是 "**Too Long; Didn't Read**"。
```shell
# 推荐安装（Rust实现）
$ brew install tealdeer
tldr --version
tealdeer 1.7.2
```
## 系统优化
### [Mos](https://mos.caldis.me/)
推荐使用[Homebrew](https://brew.sh/)来安装:
```shell
# 安装
$ brew install --cask mos
# 更新
$ brew update
$ brew reinstall mos
```
### [Mac Mouse Fix](https://macmousefix.com/)
优化`Command`+鼠标滚轮的体验
### [iTerm2](https://iterm2.com/)
### [Rectangle](https://rectangleapp.com/)
免费开源的Mac分屏工具
## 客户端
安装软件前先通过终端启用“任何来源”（推荐）
```shell
sudo spctl --master-disable
```
### [Orbstack](https://orbstack.dev/)
```shell
ssh -i ~/.orbstack/ssh/id_ed25519 -p 32222 joeljhou@centos9@orb
ssh -i ~/.orbstack/ssh/id_ed25519 -p 32222 joeljhou@ubuntu24@orb
ssh -i ~/.orbstack/ssh/id_ed25519 -p 32222 joeljhou@rocky9@orb
```
### [Sublimetext](https://www.sublimetext.com/)
文本编辑器
### IDEA（Jetbrains系列）
[官网](https://www.jetbrains.com/idea/)
IntelliJ IDEA 是一个强大的 IDE，广泛用于 Java 开发和其他多种编程语言。
### VS Code
[官网](https://code.visualstudio.com/)
Visual Studio Code 是一个轻量级的代码编辑器，支持多种编程语言并具有丰富的插件生态。
### Sourcetree
[官网](https://www.sourcetreeapp.com/)
简洁而强大的Git图形界面，免费。
### SSH客户端
SecureCRT Mac
https://termius.com/
### Navicat
* 投毒： https://www.bilibili.com/video/BV1Su411X7cL
### LuLu
开源防火墙
* 官网： https://objective-see.org/products/lulu.html
* up主推荐： https://www.bilibili.com/video/BV1ES4y1Q7BD
### MySQL 5.7
* 服务器版本: `5.7.44`
```shell
docker volume create mysql57_data

docker run -d \
  --name mysql5.7 \
  --platform linux/amd64 \
  --restart unless-stopped \
  -e TZ=Asia/Shanghai \
  -e MYSQL_ROOT_PASSWORD=root \
  -p 3307:3306 \
  -v mysql57_data:/var/lib/mysql \
  mysql:5.7
```
### MySQL 8
```shell
# 创建 MySQL 数据卷（仅需执行一次）
docker volume create mysql8_data

# 运行 MySQL 8 容器
docker pull mysql:8.0.42
docker run -d \
  --name mysql8 \
  --restart unless-stopped \
  -e TZ=Asia/Shanghai \
  -e MYSQL_ROOT_PASSWORD=root \
  -p 3306:3306 \
  -v mysql8_data:/var/lib/mysql \
  mysql:8.0.42
```

### Redis7
```shell
# 创建 Redis 数据卷（只需执行一次）
docker volume create redis7_data

# 拉取 Redis 7 官方镜像
docker pull redis:7

docker run -d \
  --name redis7 \
  --restart unless-stopped \
  -e TZ=Asia/Shanghai \
  -p 6379:6379 \
  -v redis7_data:/data \
  redis:7 \
  redis-server --appendonly yes --requirepass 123456
```
### keepass

