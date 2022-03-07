---
title: Docker详解
date: 2021-11-24 21:20:46
tags: 虚拟化
categories: 常用工具
cover: https://tva1.sinaimg.cn/large/008i3skNly1gwyfm44hg3j30dm09uq2y.jpg
---

# 引言

## 前提知识

**必须：**熟悉Linux命令和相关背景知识

**建议：**Git相关知识，git，pull

## 定位和范围

- Java开发：

  语言：java

  Spring MVC/Spring Boot/MyBatis······

- Docker开发

  语言：Go

  Swarm/Compose/Machine/mesos/k8s·······

## 为什么需要Docker

​       一款产品从开发到上线，从操作系统，到运行环境，再到应用配置。作为开发和运维之间的协作，我们需要考虑更多的问题。特别是各种版本迭代后，不同版本环境的兼容等等都是我们所需要考虑的。

​       Docker之所以发展如此迅速，是因为它对以上的问题给出了一个标准化的解决方案。

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gwlxroc3yuj317c0jqdi9.jpg" style="zoom: 50%;" />

​       形象的说：**项目带着环境一起走！**

​       总结一句话：**一次构建，处处运行！**

## Docker理念

- Docker是基于Go语言实现的云开源项目。
- Docker的主要目标是：“Build，Ship and Run Any App，Anywhere”，也就是通过对应用组件的封装、分发、部署、运行等生命周期的管理，使用户的APP（可以是一个Web应用或者是一个数据库应用）及其运行环境能够做到**“一次封装，到处运行”**。

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gwlxyhytg0j311e0gojt6.jpg" style="zoom:50%;" />

**Docker：解决了运行环境的配置问题软件容器，方便做持续集成并有助于整体发布的容器虚拟化技术。**

## 对比

### 之前的虚拟机技术

​       虚拟机（Virtual Machine）就是一种带环境安装的一种解决方案。它可以在一种操作系统里面运行另一种操作系统，比如在Windows系统里面运行Linux系统，应用程序对此毫无感知，因为虚拟机看上去跟真实系统一模一样，而对于底层系统来说，虚拟机就是一个普通的文件，不需要了就删掉，对其他部分毫无影响。这类虚拟机完美的运行了另一套系统，能够使应用程序、操作系统和硬件三者之间的逻辑不变。

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gwly97ey8oj30dm0iw0t5.jpg" style="zoom: 33%;" />

**缺点：**1、资源占用多   2、冗余步骤多   3、启动慢

### 容器虚拟机技术

​       由于之前的虚拟机存在这些缺点，Linux发展出现了另一种虚拟化技术：Linux容器。

​       **Linux容器不是模拟一个完整的操作系统，而是对进程进行隔离。**有了容器，就可以将软件运行所需要的所有资源打包到一个隔离的容器中。容器和虚拟机不同，不需要捆绑一整套操作系统，只需要软件工作所需的库资源和配置。系统因此而变得高效轻量并保证部署在任何环境中的软件都能始终如一地运行。

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gwmjtch6bcj30fe0joaap.jpg" style="zoom:33%;" />

 **Docker和传统虚拟化方式的不同：**

- 传统虚拟化技术是虚拟出一套硬件后，在其上运行一个完整的操作系统，在该系统上再运行所需要的应用进程。
- 而容器内的应用进程直接运行于宿主的内核，容器内没有自己的内核，而且也**没有进行硬件虚拟**。因此容器**更为轻便**。
- 每个容器之间**互相隔离**，每个容器都有自己的文件系统，容器之间进程不会互相影响，能区分计算资源。

# Docker安装

## 前提说明

CentOs Docker 安装：

- 仅支持以下的CentOs版本：CentOs 6.5（64-bit）或更高的版本、CentOs 7（64-bit）

前提条件：

- 目前，CentOs 仅发行版本中的内核支持Docker
- Docker运行在CentOs 7上，要求系统为64位，系统内核版本为3.10以上
- Docker运行在CentOs 6.5或更高版本上，要求系统为64位，系统内核版本为2.6.32-431或更高版本

查看自己的内核：

​       uname命令用于打印当前系统的相关信息（内核版本号、硬件架构、主机名称和操作系统类型等等）

## Docker 的基本组成

- 镜像（image）

​       Docker 镜像（Image）就是一个**只读**的模版。镜像可以用来创建Docker容器，**一个镜像可以创建很多个容器**。

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gwmkpi8o55j30y808uaai.jpg" style="zoom:33%;" />

- 容器（container）

​       Docker利用容器（Container）独立运行的一个或一组应用。**容器是用镜像创建的运行实例。**它可以被启动、开始、停止、删除。每一个容器都是相互隔离的、保证安全的平台。

​       **可以把容器看做成一个简易版的Linux环境**（包括root用户权限、进程空间、用户空间和网络空间等）和运行在其中的应用程序。

​       容器的定义和镜像几乎一模一样，也是一堆层的统一视角，唯一区别在于容器的最上面那一层是可读可写的。

- 仓库（repository）

​       仓库（Repository是**集中存放镜像文件**的场所。 

​       仓库（Repository）和仓库注册服务器（Registry）是有区别的，仓库注册服务器上往往存放着多个仓库，每一个仓库中又包含了多个镜像，每一个镜像有不同的标签（tag）。

​       仓库分为公开仓库（Public）和私有仓库（Private）两种形式。**最大的公开仓库是Docker Hub（https://hub.docker.com/）**，存放了数量庞大的镜像供用户下载。国内的公开仓库包括阿里云、网易云等。

- Docker架构图

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gwmkiawzr6j31e00r8jux.jpg" style="zoom: 33%;" />

**总结：**Docker本身是一个容器运行载体或称之为管理引擎。我们把应用程序和配置依赖打包好形成一个可交付的运行环境，这个打包好的运行环境就似乎image镜像文件。只有通过这个镜像文件才能生成Docker容器。image文件可以看做是容器的模版。Docker根据image文件生成容器的实例。同一个image文件，可以生成多个同时运行的容器实例。

## 安装步骤

https://www.runoob.com/docker/macos-docker-install.html

查看Docker版本：`docker --version`

## 镜像加速

- 阿里云镜像加速

​       进入阿里云官网，登录后进入容器镜像服务—>选择镜像工具—>镜像加速器—>根据对应系统进行配置操作

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gwmtfkqws9j31o00n0tby.jpg" style="zoom:33%;" />

​       重启后终端输入`docker info`验证是否配置成功。

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gwmtj1y8fqj30lc04wmxe.jpg" style="zoom: 50%;" />

- 网易云镜像加速

` "registry-mirrors": [
    "http://hub-mirror.c.163.com"
  ]`

## Hello World

​       命令`docker run hello-world`

# 运行底层原理

## Docker是怎么工作的

​       Docker是一个**Client-Server结构**的系统，Docker守护进程运行在主机上，然后通过Socket连接，从客户端访问，守护进程从客户端接受命令并管理运行在主机上的容器。

​       **容器，是一个运行时环境**，就是我们鲸鱼背上的集装箱。

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gwmz5o2m6uj30y60q20uw.jpg" style="zoom: 25%;" />

## 为什么docker比vm快

- Docker有着比虚拟机更少的抽象层。由于Docker不需要Hypervisor实现硬件资源的虚拟化，运行在Docker容器上的程序**直接使用的都是实际物理机的硬件资源**。因此，在CPU、内存利用率上Docker将会在效率上有明显的优势。

- Docker利用的是宿主机的内核，而不需要Guest OS。因此，当新建一个容器时，Docker不需要和虚拟机一样重新加载一个操作系统内核，从而可以避免寻找、加载操作系统内核这个费时费资源的过程。当新建一个虚拟机时，虚拟机软件需要加载Guest OS，返回新建过程是分钟级别的，而Docker由于直接利用宿主机的操作系统，则省略了返回过程，。因此，**新建一个Docker容器只需要几秒钟**。

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gwmyq4804vj30rg0imgnn.jpg" style="zoom:33%;" />

- **对比！（重要）**

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gwmyqoljw5j311u0hkdi6.jpg" style="zoom: 25%;" />

# Docker 常用命令

## 帮助命令

- **`docker version`：查看版本**
- **`docker info`：查看详细信息**
- **`docker --help`：查看帮助**  

## 镜像命令

- **`docker images`：列出本地主机上的镜像**

​       REPOSITORY：镜像的仓库源；TAG：镜像的标签；IMAGE ID：镜像ID；CREATED：镜像创建的时间；VIRTUAL SIZE：镜像大小

​       （1）-a：列出本地所有镜像（含中间映像层）

​       （2）-q：只显示镜像ID

​       （3）--digests：显示镜像的摘要信息

​       （4）--no-trunc：显示完整的镜像信息

​       **我们使用`Repository:Tag`来表示一个镜像。**

- **`docker search 某个镜像的名称`：搜索镜像**

​       （1）--no-trunc：显示完整的镜像描述

​       （2）-s：列出收藏数不小于指定值的镜像

​       （3）--automated：只列出automated build类型的镜像

- **`docker pull 某个镜像的名称[:TAG]`：下载镜像**

- **`docker rmi 某个镜像名称的ID`：删除镜像**

​       （1）删除单个：`docker rmi -f 镜像ID`

​       （2）删除多个：`docker rmi -f 镜像名1:TAG 镜像名2:TAG`

​       （3）删除全部：`docker rmi-f $(docker images -qa)`

## 容器命令

- **`docker run [options] image [command][arg...]`：新建并启动交互式容器**

​       options说明（常用）：

​              --name="容器的新名字"：为容器指定一个名称；

​              **-d：后台运行容器，并返回容器ID，也即启动守护式容器**

​              **-i：以交互模式运行容器，通常与-t同时使用**

​              **-t：为容器重新分配一个伪输入终端，通常与-i同时使用**

​              **-P：随机端口映射**

​              **-p：指定端口映射**，有以下4种形式：

​                     ip:hostPort:containerPort

​                     ip::containerPort

​                     **hostPort:containerPort**

​                     containerPort

- **`docker ps [options]`：列出当前所有正在运行的容器**

​       options说明（常用）：

​              -a：列出当前所有正在运行的容器+历史上运行过的

​              -l：显示最近创建的容器

​              -n：显示最近n个创建的容器

​              **-q：静默模式，只显示容器编号**

​              --no-trunc：不截断输出

- **`exit`：容器停止退出**
- **`ctrl+P+Q`：容器不停止退出**
- **`docker start 容器ID或者容器名`：启动容器**
- **`docker restart 容器ID或者容器名`：重启容器**
- **`docker stop 容器ID或者容器名`：停止容器**
- **`docker kill 容器ID或者容器名`：强制停止容器**
- **`docker rm 容器ID`：删除已停止的容器**

​       一次性删除多个容器：

​              `docker rm -f $(docker ps -aq)`

​              `docker ps -aq | xargs docker rm`

**重要：**

​       （1）**`docker run -d 容器名`：启动守护式容器**

​       （2）**`docker logs -f -t --tail 容器ID`：查看容器日志**

​              -t：加入时间戳

​              -f：跟随最新的日志打印

​              --tail 数字：显示最后多少条

​       （3）**`docker top 容器ID`：查看容器内运行的进程**

​       （4）**`docker inspect 容器ID`：查看容器内部细节**

​       （5）进入正在运行的容器并以命令行交互：

​						**`docker exec -it 容器ID bashShell`**

​						**`docker attach 容器ID`**

​				区别：attach：直接进入容器启动命令的终端，不会启动新的进程；

​							exec：在容器中打开新的终端，并且可以启动新的进程

​       （6）**`docker cp 容器ID:容器内路径 目的主机路径`：从容器内拷贝文件到主机上**

# Docker 镜像

## 是什么

### UnionFS（联合文件系统）

​       Union文件系统（UnionFS）是一种分层、轻量级并且高性能的文件系统，它支持**对文件系统的修改作为一次提交来一层层的叠加**，同时可以将不同目录挂载到同一个虚拟文件系统下。

​       Union文件系统时Docker镜像的基础。镜像可以通过分层来进行继承，基于基础镜像（没有父镜像），可以制作各种具体的应用镜像。

​       **特性：**一次同时加载多个文件系统，但从外面看起来，只能看到一个文件系统，联合加载会把各层文件系统叠加起来，这样最终的文件会包含所有底层的文件和目录。

### Docker 镜像加载原理

​       Docker的镜像实际上由一层一层的文件系统组成，这种层级的文件系统是UnionFS。

​       bootfs（boot file system）主要包含bootloader和kernel，bootloader主要是引导加载kernel，Linux刚启动时会加载bootfs文件系统，在Docker镜像的最底层就是bootfs。这一层与我们典型的Linux/Unix系统是一样的，包含boot加载器和内核。当boot加载完成之后，整个内核就都在内存中了，此时内存的使用权已经由bootfs转交给内核，此时系统也会写在bootfs。

​       rootfs（root file system），在bootfs之上，包含的就是典型Linux系统中的/dev，/proc，/bin，/etc等标准目录和文件。rootfs就是各种不同的操作系统的发行版，比如Ubuntu，CentOs等等。

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gwnn7ysl4jj30pi0f0q4g.jpg" style="zoom: 33%;" />

**问题：**平时我们安装进虚拟机的CentOs都是好几个G，为什么Docker这里才只需要200M？

**答：**对于一个精简的OS，rootfs可以很小，只需要包括最基本的命令、工具、和程序库就可以了，因为底层直接使用的是Host的Kernel，自己只需要提供rootfs就行了。由此可见，对于不同的Linux发行版，bootfs基本是一致的，rootfs会有差别，因此不同的发行版可以公用bootfs。

### 分层的镜像

​       镜像，**一层一层下载**，包含了运行时所需的全部环境。

​       例如，Tomcat，为什么有400多M，因为里面可能包含centos，jdk8，tomcat等。

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gwnnjlxajvj310k0hugnv.jpg" style="zoom: 25%;" />

### 为什么采用这种分层结构

​       最大的一个好处——**共享资源**。

​       例如，有多个镜像都从相同的base镜像构建而来，那么宿主机只需在磁盘上保存一份base镜像，同时内存中也只需要加载一份base镜像，就可以为所有的容器服务了，同时，**镜像的每一层都可以被共享**。

## 特点

- Docker镜像都是只读的
- 当容器启动时，一个新的可写层被加载到镜像的顶部
- 这一层通常被称作“容器层”，“容器层”之下的都叫“镜像层”

## Docker镜像commit操作

- docker commit 提交容器副本使之成为一个新的镜像
- `docker -m="提交的描述信息" -a="作者" 容器ID 要创建的目标镜像名:[标签名]`

**案例演示：**

1、从Hub上下载Tomcat镜像到本地并成功运行

​		`docker run -it -p 8080:8080 tomcat`

​					-p：主机端口：docker容器端口

​					-P：随机分配端口

​					-i：交互

​					-t：终端

2、故意删除上一步镜像产生的tomcat容器的文档

​			进入webapps目录下，删除docs

3、当前运行的tomcat是一个没有文档内容的容器，以它为模版，commit一个新镜像

4、启动新镜像和原来的进行对比

​			新镜像：没有docs

​			原来的：有docs

# Docker 容器数据卷

## 是什么

​	   先来看看Docker的理念：

- 将应用与运行时环境打包形成容器运行，运行可以伴随着容器，但是我们对数据的要求希望是持久化的
- 容器之间希望有可能共享数据

​	   Docker容器产生的数据，如果不通过docker commit生成新的镜像，使得数据成为镜像的一部分保存下来，那么当容器删除后，数据自然也就没有了。

​       **为了能够保存数据，我们在Docker中使用卷。**

## 能干什么

- **容器的持久化**
- **容器间继承+共享数据**

​       卷就是目录或文件，存在于一个或多个容器中，由Docker挂载到容器，但不属于联合文件系统，因此能够绕过Union File System提供一些用于**持续存储或共享数据**的特性。

​       卷的设计目的就是数据的持久化，**完全独立于容器的生命周期**，因此，Docker不会在容器删除时删除其挂载的数据卷。

​       特点：

- 数据卷可在容器之间共享或重用数据
- 卷中的更改可以直接生效
- 数据卷的更改不会包含在镜像的更新中
- 数据卷的生命周期一直持续到没有容器使用它为止

## 添加数据卷

（1）**直接命令添加**

- 添加：**`docker run -it -v /宿主机绝对路径目录:/容器内目录 镜像名`**
- 查看数据卷是否挂载成功：`docker inspect 容器ID` 查看Volumes
- 容器和宿主机之间数据共享
- 容器停止退出后，主机修改后数据是否同步：是
- 命令（带权限）：`docker run -it -v /宿主机绝对路径目录:/容器内目录:ro 镜像名`

（2）**DockerFile添加**

- 根目录下新建mydocker文件夹并进入
- 可在DockerFile中使用VOLUME指令来给镜像添加一个或多个数据卷

 **`VOLUME["/dataVolumeContainer","/dataVolumeContainer2","/dataVolumeContainer3"]`**

​       **说明：**出于可移植和分享的考虑，用`-v /宿主机绝对路径目录:/容器内目录`不能够直接在DockerFile中实现。

- File构建：例如 

```# volume test
FROM centos
VOLUME ["/dataVolumeContainer1","/dataVolumeContainer2"]
CMD echo "finished,----------success"
CMD /bin/bash```
```

- build后生成镜像
- run容器
- 对应主机目录地址：会有一个默认的地址

## 数据卷容器

​       **定义：**命名的容器挂载数据卷，其他容器通过挂载这个（父容器）实现数据共享，挂载数据卷的容器，称之为数据卷容器。

​       **使用`--volumes-from`进行容器间的传递共享**

- 先启动一个父容器dc01
- 再启动dc02/dc03继承自dc01

​       **结论：容器之间配置信息的传递，数据卷的生命周期一直持续到没有任何容器使用它为止。**

# DockerFile解析

## 是什么

​       DockerFile是用来构建Docker镜像的构建文件，是由一系列命令和参数构成的脚本。

​       **构建三步骤：**（1）编写DockerFile文件（2）`docker build`（3）`docker run`

## DockerFile构建过程解析

（1）DockerFile内容基础知识

- 每条保留字指令都必须为大写字母，且后面要跟随至少一个参数
- 指令按照从上到下，顺序执行
- #表示注释
- 每条指令都会创建一个新的镜像层，并对镜像进行提交

（2）Docker执行DockerFile的大致流程

- docker从基础镜像运行一个容器
- 执行一条指令并对容器作出修改
- 执行类似docker commit的操作提交一个新的镜像层
- docker再基于刚提交的镜像运行一个新容器
- 执行DockerFile中的下一条执行，直到所有指令都执行完成

**总结：**

​       从应用软件角度来看，DockerFile、Docker镜像与Docker容器分别代表软件的三个不同阶段：

- DockerFile是软件的原材料
- Docker镜像是软件的交付品
- Docker容器则可以认为是软件的运行态

​       DockerFile面向开发，Docker镜像成为交付标准，Docker容器则涉及部署与运维，三者缺一不可，合力充当Docker体系的基石。

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gwr765ou85j310a0faabe.jpg" style="zoom:33%;" />

- DockerFile，需要定义一个DockerFile，DockerFile定义了进程需要的一切东西。DockerFile涉及的内容包括执行代码或者是文件、环境变量、依赖包、运行时环境、动态链接、操作系统的发行版、服务进程和内核进程（当应用程序需要和系统服务和内核进程打交道，这时需要考虑如何设计namespace的权限控制）等等。
- Docker镜像，在用DockerFile定义了一个文件后，docker build时就会产生一个Docker镜像，当运行Docker镜像时，会真正开始提供服务。
- Docker容器，容器时直接提供服务的。

## DockerFile 保留字

- FROM：基础镜像，当前镜像是基于哪个镜像的
- MAINTAINER：镜像维护者的姓名和邮箱地址
- RUN：容器构建时需要运行的命令
- EXPOSE：当前容器对外暴露出的端口号
- WORKDIR：指定在创建容器后，终端默认登陆进来的工作目录，一个落脚点
- ENV：用来构建镜像过程中设置环境变量
- ADD：将宿主机目录下的文件拷贝进镜像且ADD命令会**自动处理URL和解压tar压缩包**
- COPY：类似ADD，拷贝文件和目录到镜像中
- VOLUME：容器数据卷，用于数据保存和持久化工作
- CMD：指定一个容器启动时要运行的命令。DockerFile中可以有多个CMD指令，但只有最后一个生效，CMD会被docker run之后的参数替换
- ENTRYPOINT：指定一个容器启动时要运行的命令。指定容器启动程序和参数。docker run之后的参数会传递给ENTRYPOINT，之后形成新的命令组合
- ONBUILD：当构建一个被继承的DockerFile时运行命令，父镜像在被子镜像继承后，父镜像的onbuild被触发

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gwr834pqccj316e0so41d.jpg" style="zoom: 25%;" />

# Docker 常用安装

## 总体步骤

- 搜索镜像
- 拉取镜像
- 查看镜像
- 启动镜像
- 停止容器
- 移除容器

## 安装 Tomcat

- docker hub上查找tomcat镜像：`docker search tomcat`
- 从docker hub上拉取tomcat镜像到本地：`docker pull tomcat`
- `docker images` 查看是否有拉取到Tomcat
- 使用Tomcat镜像创建容器：`docker run -it -p 8080:8080 tomcat`

## 安装 Mysql

- docker hub上查找mysql镜像：`docker search mysql:5.6`
- 从docker hub上拉取mysql镜像到本地，标签为5.6：`docker pull mysql:5.6`
- 使用mysql5.6镜像创建容器：

​       （1）使用mysql镜像：`docker run -p 12345:3306 --name mysql -v xxx -v xxx -e MYSQL_ROOT_PASSWORD=123456 -d mysql:5.6`

​       （2）外部系统也来连接运行在docker上的mysql服务

​       （3）数据备份（导出数据库表）：`docker exec mysql服务容器ID sh -c 'exec mysqldump --all-databases -uroot -p"123456"'>/lwq/all-databases.sql`

## 安装Redis

- docker hub上查找redis镜像：`docker search redis:3.2 `
- 从docker hub上拉取redis镜像到本地，标签为3.2：`docker pull redis:3.2`
- 使用redis 3.2镜像创建容器：

​       （1）使用redis镜像：`docker run -p 6379:6379 -v xxx -v xxx -d redis:3.2 redis-server /usr/local/etc/redis/redis.conf --appendonly yes`

​       （2）在主机/xxx/myredis/conf/redis.conf 目录下新建redis.conf文件：`vim /xxx/myredis/conf/redis.conf/redis.conf`

- `docker exec -it 运行着的Redis服务容器的ID redis-cli`
- 退回宿主机，查看持久化文件appendonly.aof是否生成

# 本地镜像发布到阿里云

​        **流程：**

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gwyenzug57j310u0twmz0.jpg" style="zoom: 33%;" />

- 镜像生成

​       （1）编写DockerFile文件

​       （2）从容器创建一个新的镜像：`docker commit [options] 容器ID [repository[:tag]]`

- 本地镜像推送到阿里云

​       （1）本地镜像素材原型

​       （2）阿里云开发者平台——`https://dev.aliyun.com/search.html`

​       （3）创建仓库镜像：命名空间+仓库名称

​       （4）将镜像推送到registry（阿里云网站上有过程，点击仓库管理）

​       （5）公有云可以查询到 

​       （6）查看详情

- 将阿里云上的镜像下载到本地

