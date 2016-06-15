# Docker

[Docker](https://www.docker.com) 是一款基于 Linux Containers 技术构建的开源容器引擎，用来将应用程序及其所有依赖打包进一个标准化单元，即所谓的“容器”。Docker 利用 Linux 内核的资源隔离特性，允许独立的容器在一个 Linux 实例中运行，从而避免启动和管理虚拟机的巨大开销。

有关 Docker 的详细介绍和产品优势，请参考官方文档，也可以在网络上找到大量有关 Docker 的有趣资料。

在这里介绍使用 Docker 模式来搭建开发环境，主要目的是为了最大程度上保持环境的统一，尤其是当企业内部既有 OS X 用户还有 Windows 用户，且最终发布上线的系统又是运行在 Linux 上的时候，就显得非常有意义了。Docker 能够有效解决“为什么在我这里可以运行，但在你们那里就不可以”等类似的问题。

**注：截止到文档编写时，Docker 的最新版本为 1.9.1。以下的安装过程均基于此版本验证通过。**

在这里，我们只介绍在桌面系统上搭建 Docker 开发环境的具体方法。

## 准备

在正式开始介绍如何搭建 Docker 环境之前，需要先了解一些有关 Docker 生态系统的知识。

### Docker Engine

Docker 平台的核心是 [Docker Engine](https://www.docker.com/docker-engine)。Docker Engine 是一个轻量级的运行时环境和一个可以构建与运行 Docker 容器的强健工具。有关 Docker Engine 的架构和工具的详细使用方法请参考[官方文档](https://docs.docker.com/)。

在前面的介绍中说过，Docker 是一种容器技术，可以免除使用虚拟机所带来的开销。理论上讲，应该就不再需要安装像 VirtualBox 这类的虚拟机软件了。然而问题在于 Docker 是基于 LXC 技术实现的，也就是说只有支持 LXC 的 Linux 系统才能原生的运行 Docker。很显然 OS X 和 Windows 都不是 Linux，自然无法直接运行 Docker，所以才需要借助第三方虚拟化软件，在这些系统上虚拟一个 Linux 环境出来。其实不只是 VirtualBox，Docker 还支持[很多](https://docs.docker.com/machine/drivers/)虚拟化技术。

### Docker Machine

[Docker Machine](https://www.docker.com/docker-machine) 顾名思义，就是用来管理运行有 Docker 环境的计算机的，只不过这里的计算机并不局限于电脑、还包括云服务和数据中心等。Docker Machine 会自动准备主机、在上面安装 Docker Engine、并配置与 Docker Engine 通讯的 Docker 客户端。

因为 OS X 和 Windows 系统并不能直接支撑 Docker 运行，因此 Docker Machine 在这里的作用可以简单理解为是通过虚拟化软件创建一个虚拟机，并安装好一个含有 Docker Engine 的小型 Linux 操作系统，并提供简单的办法让 Docker 客户端工具可以与这个虚拟机中运行的 Docker Engine 通讯。这样一来，看上去就好像是 Docker 直接运行在这些系统上一样。

### Docker Compose

通常一个复杂的应用程序是由若干更小的系统组合在一起工作的。例如一个标准的管理信息系统通常会由反向代理服务器、应用程序服务器和数据库服务器等组成，它们一并协同工作才能完成预定的功能。Docker 在实际工作时会将这些系统转换为相互关联的独立容器。为了避免构建、运行和管理每一个独立的容器，[Docker Compose](https://www.docker.com/docker-compose) 可以通过配置文件定义一个多容器的应用程序，并制定各容器之间的依存关系，之后就可以使用一条命令来启动该程序了。应用程序的结构和配置是被统一管理的，使得编排起这样的一套应用变得简单而可以重复。

## 在 OS X 系统上部署 Docker 环境

Docker 官方网站发布有 [Docker Toolbox](https://www.docker.com/docker-toolbox) 工具包，可以方便安装 Docker 所需要的各种软件及工具，但是为了能更好的理解 Docker 生态环境的构成以及运行方式，我们还是使用 Homebrew 来完成安装。

**提示：在此之前，请确保系统中已经安装了 Homebrew 和 Cask，并且可以正常运行。有关安装的具体方法，请参见搭建开发环境下的 [OS X](./osx.md) 章节。**

### 安装 VirtualBox

可以通过 Cask 来安装 VirtualBox：

```bash
$ brew cask install virtualbox
```

安装完成以后，VirtualBox 程序会出现在 Applications 文件夹中，但是我们在下面的介绍中不会直接使用这个程序。

### 安装 Docker Engine

使用 Homebrew 可以直接安装 Docker Engine：

```bash
$ brew install docker
```

安装完成以后，可以使用 `docker` 命令作为客户端工具来与 Docker Engine 通讯，完成管理 Docker 容器的工作。

### 安装 Docker Machine

同样的，Homebrew 也可以直接安装 Docker Machine：

```bash
$ brew install docker-machine
```

安装完成以后，可以使用 `docker-machine` 命令来管理运行有 Docker 环境的虚拟机。

### 安装 Docker Compose

```bash
$ brew install docker-compose
```

安装完成以后，可以使用 `docker-compose` 命令来编排 Docker 容器的运行。

### 编译与运行

#### 创建 Linux 虚拟环境

使用 Docker Machine 来创建一个 Linux 虚拟环境：

```bash
$ docker-machine create -d virtualbox dev
```

各参数的含义如下：

* `create` 是指调用创建虚拟机的子命令；
* `-d virtualbox` 指定了虚拟机的驱动为 VirtualBox，等价于 `--driver` 参数；
* `dev` 是虚拟机的名字，后面需要引用该虚拟机的时候，都可以使用这个名字。

如果是第一次运行此命令，会从网络上下载一个名为 `boot2docker.iso` 的文件，这个文件大约有 30M 大小，是一个轻量级的 Linux 发行版，完全在内存中运行，并预置了 Docker Engine 环境。该文件下载完成以后，Docker Machine 会创建名为 dev 的虚拟机，并加载 `boot2docker.iso` 文件以启动系统，这个系统的名字就叫做 Boot2Docker。

#### 连接 Docker Engine

Boot2Docker 系统是运行在虚拟机中的，如果宿主机中的客户端想要访问虚拟机中的 Docker Engine，还需要进行一下配置：

```bash
$ eval "$(docker-machine env dev)"
```

`docker-machine env dev` 命令是用来输出一些有关连接 dev 虚拟机的环境变量到控制台的。`eval` 命令就是在当前控制台设置这些环境变量。以上命令执行完成以后就可以使用 `docker` 命令来进行操作了，此时就好像 Docker Engine 是运行在宿主机上的一样。其实还可以在宿主机中启动多个虚拟 Linux 环境，以便隔离不同的环境。同样可以用上面的命令来切换 Docker 客户端具体连接哪一台虚拟机。

比如我们可以继续创建另外一台虚拟机，并切换连接：

```bash
$ docker-machine create -d virtualbox prod
$ eval "$(docker-machine env prod)"
```

#### 下载 cdeio-docker 项目

`cdeio-docker` 项目是一个有关 CDE.IO 平台的 Docker 容器项目，里面包含了以 Docker 方式运行系统所需的全部项目源代码和配置文件，方便快速编译和启动系统。

使用以下命令来下载 cdeio-docker 项目的源代码：

```bash
$ git clone https://github.com/zyeeda/cdeio-docker
$ cd cdeio-docker
$ git submodule update --init
```

### 编译 origin 项目

首先将连接的虚拟机切换到 dev 上：

```bash
$ eval "$(docker-machine env dev)"
```

在 `cdeio-docker` 项目下存在一个 `docker-compose.yml` 文件，这个文件定义了若干容器的描述信息，使用 `docker-compose` 命令就可以直接进行编译：

```bash
$ docker-compose run origin mvn clean install
```

`run origin` 是指运行 `docker-compose.yml` 配置文件中名为 `origin` 的容器，并在这个容器内运行 `mvn clean install` 命令执行编译。第一次运行此命令的时候，会从网络上下载包含有编译环境的 Docker 镜像，有关 Docker 镜像的相关内容，请参考[官方文档](http://docs.docker.com/engine/userguide/image_management/)。

#### 编译 cdeio-runtime 项目

依然是在 `cdeio-docker` 目录下运行（下同）：

```bash
$ docker-compose run runtime mvn clean install
```

#### 编译 colorvest 项目

```bash
$ docker-compose run colorvest make clean compile
```

#### 编译 cdeio-samples 项目

```bash
$ docker-compose run server mvn clean package
```

#### 运行 cdeio-samples 系统

在开始运行系统之前，需要先记录一下虚拟机的 IP 地址，以便系统成功启动以后可以在浏览器中访问：

```bash
$ docker-machine ip dev
```

这条命令的含义就是输出名为 dev 的虚拟机的 IP 地址，记录下此 IP 地址，在这里我们假定其输出为 `192.168.99.100`。

然后执行以下命令：

```bash
$ docker-compose --service-ports run server
```

**注意：这里要添加 `--service-ports` 参数，否则虚拟机中的端口无法暴露，也就无法对外提供服务。**

查看输出日志，待显示系统成功启动以后，就可以打开浏览器，使用上面记录的 IP 地址来访问了，例如：http://192.168.99.100:8080/cdeio-samples。

## 在 Windows 系统上部署 Docker

同样的，我们使用 Chocolatey NuGet 来安装 Docker 所需的软件。

**注意：需要以管理员身份打开命名提示符窗口来执行以下命令。关于 Chocolatey NuGet 的具体安装和使用方法请参见搭建开发环境下的 [Windows](./windows.md) 章节。**

### 安装 VirtualBox

```cmd
C:\Users\zyeeda> choco install virtualbox -y
```

### 安装 Docker Engine

```cmd
C:\Users\zyeeda> choco install docker -y
```

### 安装 Docker Machine

```cmd
C:\Users\zyeeda> choco install docker-machine -y
```

### 安装 Docker Compose

```cmd
C:\Users\zyeeda> choco install docker-compose -y
```