# OS X

**注：在搭建环境之前，建议先将 OS X 系统升级到最新版本。以下介绍如无特别说明均是在 El Capitan 系统下进行的。**

## 安装软件环境

### 安装 Homebrew

[Homebrew](http://brew.sh/) 是 OS X 系统上的包管理工具，使用它可以方便安装和管理所需要的其它软件。

Homebrew 的安装非常简单，只需要在 Terminal 窗口下执行命令：

```bash
$ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

第一次使用 Homebrew 安装软件之前，最好先执行 `brew doctor` 命令检查系统当前配置是否满足运行 Homebrew 的要求，如果不满足需要根据此命令提示的信息进行修改。

Homebrew 的常用命令如下：

* `brew list` 列出当前已经安装的软件包
* `brew search <keyword>` 按照 keyword 在 Homebrew 的软件库中检索软件包
* `brew install <package>` 安装指定名称的软件包
* `brew uninstall <package>` 卸载指定名称的软件包
* `brew info <package>` 查询指定名称的软件包的信息
* `brew update` 更新 Homebrew 本地软件包索引库
* `brew upgrade` 升级所有已经安装的软件包

因此，如需升级已经安装的软件，可以执行如下命令：

```bash
$ brew update && brew upgrade
```

### 安装 Homebrew Cask

[Cask](http://caskroom.io/) 是 Homebrew 的一种扩展机制，可以简洁、优雅的安装一些较大型的二进制应用程序，比如 Google Chrome 或者 Oracle VirtualBox 之类的。可以使用以下命令安装 Cask：

```bash
$ brew install caskroom/cask/brew-cask
```

### 安装 Git

[Git](https://git-scm.com/) 是被社区广泛使用的开源版本管理工具，使用 Git 来管理的最著名的项目当属 Linux 内核。有关 Git 的使用方法请自行搜索相关文档。值得一提的是，基于 Git 工具产生了一个以代码协作开发为基础的社交网站，即大名鼎鼎的 [Github](https://github.com/)，由此也催生了社会化编程的新模式。可以用 Homebrew 来直接安装 Git：

```bash
$ brew install git
```

### 安装 Node.js

CDE.IO 一些源代码（CoffeeScript 代码）的编译需要使用 Node.js 环境。Node.js 是基于 Google Chrome V8 引擎开发的 JavaScript 运行时环境，具有事件驱动和非阻塞 I/O 模型等特点，并且轻巧与高效。

Node.js 的版本迭代非常快，为了能够更好的在各个版本间进行切换，我们使用 [Node Version Manager](https://github.com/creationix/nvm)（nvm）来管理 Node.js。

首先需要安装 nvm：

```bash
$ brew install nvm
```

安装完成以后，Homebrew 会提示对 Bash 的配置文件进行一些修改。编辑或新建 `~/.bash_profile` 文件，然后视情况添加如下内容：

```
export NVM_DIR=~/.nvm
source $(brew --prefix nvm)/nvm.sh
```

**提示：如果由于操作失误，清空或没有看清相关提示信息的时候，可以使用 `brew info nvm` 命令重新查看。**

nvm 的一些常用命令如下：

* `nvm ls` 列出当前系统已经安装的 Node.js 版本
* `nvm ls-remote` 列出所有远程可用的 Node.js 版本
* `nvm install <version>` 安装指定的 Node.js 版本
* `nvm use <version>` 切换到指定的 Node.js 版本
* `nvm uninstall <version>` 卸载指定的 Node.js 版本

截止到文档编写时，Node.js 的最新版本是 5.2.0，因此可以用如下命令来安装：

```bash
$ nvm install 5.2.0
```

安装完成以后用下面的命令来启用：

```bash
$ nvm use 5.2.0
```

**提示：如果想要保持始终安装最新版本的 Node.js，可以使用命令 `nvm install 5`。**

### 安装 CoffeeScript

编译 [CoffeeScript](http://coffeescript.org/) 代码需要在全局安装 coffee 命令。注意，因为 CoffeeScript 语法规范的更新，CDE.IO 的代码不能使用最新版本的 coffee 命令编译，最高版本支持到 1.8.0。有关详细信息请参考 1.9.0 版本 [Change Log](http://coffeescript.org/#changelog) 的第三条。

安装方法如下：

```bash
$ npm install -g coffee-script@1.8.0
```

### 安装 JDK

可以使用 `brew cask` 命令来安装最新版本的 JDK。截止到文档编写时，最新版本是 JDK8u66。

安装方法如下：

```bash
$ brew cask install java
```

**注：在安装过程中会提示输入当前用户的密码。**

安装完成以后，需要在 Bash 的配置文件中添加 `JAVA_HOME` 环境变量。编辑或新建 `~/.bash_profile` 文件，视情况添加如下命令：

```
export JAVA_HOME=/Library/Java/Home
```

关闭 Terminal 窗口再重新打开，执行 `echo $JAVA_HOME` 命令，能够获得相同的路径输出就说明设置成功。

### 安装 Maven

[Maven](https://maven.apache.org/) 是 Apache 出品的一款项目构建管理工具，使用 Maven 可以按照统一的规范对项目进行管理，并实施构建、运行测试、生成报告和文档等。用 Homebrew 可以直接安装 Maven：

```bash
$ brew install maven
```

### 安装 MySQL

Homebrew 也可以直接安装 [MySQL](https://www.mysql.com/) 服务：

```bash
$ brew install mysql
```

安装完成以后，根据 Homebrew 的提示，要运行 `mysql_secure_installation` 命令，为 MySQL 进行安全配置，但在此之前要先启动 MySQL 服务器。

```bash
$ mysql.server start
$ mysql_secure_installation
```

然后根据提示完成配置即可。

**注：为了方便后续配置，这里将 MySQL root 用户的密码设置成 `mysecretpassword`。**

Homebrew 还提示了一些关于如何让 MySQL 开机自动运行的方法，请根据提示信息操作即可。想要手动停止 MySQL 服务器只需要运行：

```bash
$ mysql.server stop
```

**提示：同样可以使用 `brew info mysql` 来再次查看安装之后输出的提示信息。**

为了便于数据库连接，我们需要统一一下数据库服务器的本地域名。编辑 `/etc/hosts` 文件（需要输入当前用户密码），添加如下内容：

```
127.0.0.1   mysql
```

即把域名 mysql 解析为本地 IP 地址 127.0.0.1。

## 下载 CDE.IO 源代码

编译 CDE.IO 需要下载多个源代码仓库，按照顺序分别执行下面的每一条命令即可获得所需要的全部源代码，并切换到指定分支。

```bash
$ git clone -b develop https://github.com/zyeeda/origin
$ git clone -b develop https://github.com/zyeeda/cdeio-runtime
$ git clone -b develop https://github.com/zyeeda/colorvest
```

**注：命令运行结束以后，会在命令运行的目录下创建三个文件夹，分别是 `origin`、`cdeio-runtime` 和 `colorvest`。因此最好选定一个目录执行这些命令，比如 `~/Code/zyeeda`，以下命令均假设在该目录下执行。**

## 编译

### 编译 origin 项目

```bash
$ cd origin
$ mvn clean install
```

**注：第一次使用 Maven 编译项目的时候会从网络上下载很多依赖库，根据网速的不同所需时间也有所不同，请耐心等待。第一次编译完成以后，这些依赖库会被缓存起来，再次编译的时候速度就不会那么慢了。**

### 编译 cdeio-runtime 项目

```bash
$ cd cdeio-runtime
$ mvn clean install
```

**注：编译不同的项目，所需要的依赖库也不同，因此第一次编译本项目时依然会下载很多文件。**

### 编译 colorvest 项目

```bash
$ cd colorvest
$ make clean compile
```

**提示：在执行 make 命令的时候，有时会提示 `-bash: coffee: command not found` 错误，这种情况下注意检查一下是否有使用 `nvm use 5.1.0` 来启动已经安装了 coffee 命令的 Node.js 环境。**

## 运行示例项目

### 下载 colorvest-samples 项目源代码

```bash
$ git clone -b develop https://github.com/zyeeda/colorvest-samples
```

### 编译

```bash
$ cd colorvest-samples
$ mvn clean package
```

### 创建数据库

首先，登录 MySQL 服务器，并进入 MySQL 的命令提示窗口：

```bash
$ mysql -u root -p
```

按提示输入 root 用户的密码。

创建数据库：

```sql
mysql> CREATE DATABASE `cdeio-samples` DEFAULT CHARACTER SET = UTF8;
mysql> exit;
```

**注：以上 SQL 中指定的数据库名称是可以不同的，但是在变更的同时需要修改 `colorvest-samples` 项目的配置文件。具体方法，请参考开始 > 配置章节。**

导入数据库表结构：

```bash
$ mvn flyway:migrate
```

### 链接 colorvest 项目

因为 CDE.IO 平台是分模块开发的，为了让 colorvest-samples 项目能够正常运行，需要将 colorvest 项目链接到指定目录下。

```bash
$ ln -s ~/Code/zyeeda/colorvest/cdeio/ ~/Code/zyeeda/colorvest-samples/src/main/webapp/scripts/
```

**注：如果所下载的代码不在 `~/Code/zyeeda` 目录下，需要按照实际情况修改这里的路径。**

### 运行

进入 colorvest-samples 项目目录，并使用 `mvn jetty:run` 命令启动项目：

```bash
$ cd colorvest-samples
$ mvn jetty:run
```

待启动完成以后，使用浏览器访问 https://localhost:8000/cdeio-samples 即可访问系统。输入相同的用户名和密码就可以登录，开始浏览和学习项目提供的演示样例及相关代码吧。

**提示：系统成功启动以后，会出现一个 JavaScript 调试窗口，这个窗口在后续代码的开发过程中将非常有用，建议不要关闭。当启动之后第一次登录进系统，会发现浏览器卡在一个空白的页面，这时候切换到调试窗口，点一下窗口上面的 `Go` 按钮即可。这是因为该调试器拦截了服务器端 JavaScript 的执行入口。**