---
title: Maven知识详解
date: 2021-10-18 21:20:46
tags: Maven、后端
categories: 
- 后端
- 常用工具
cover: https://tva1.sinaimg.cn/large/008i3skNly1gvjs5etx30j60ri0ek0sw02.jpg
---

# Maven

​       **导言：生产环境下开发不再是一个项目一个工程，而是每一个模块创建一个工程，而多个模块整合在一起就需要使用到像 Maven 这样的构建工具。**

## Why

### 真的需要吗

​       Maven 是干什么用的? 即使不使用 Maven 我们仍然可以进行 B/S 结构项目的开发。从表述层、业务逻辑层到持久化层再到数据库都有成熟的解决方案——不使用 Maven 我们一样可以开发项目啊?

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gvgyees6jlj610y0ncgoe02.jpg" style="zoom: 33%;" />

​       这里给大家纠正一个误区，Maven 并不是直接用来辅助编码的，它战斗的岗位并不是以上各层。所以我们有必要通过企业开发中的实际需求来看一看哪些方面是我们现有技术的不足。

### 目前的技术在开发中存在的问题

- 一个项目就是一个工程

​       如果项目非常庞大，就不适合用package来划分模块。必须将项目拆分成多个工程协同开发。多个模块工程中有的是 Java 工程，有的是 Web 工程。所以，最好是每一个模块对应一个工程，利于分工协作。

- 项目中需要的jar包必须手动“复制”、“粘贴”到WEB/lib目录下

​       同样的jar包文件重复出现在不同的项目工程中，一方面浪费存储空间，另一方面会让项目显得比较臃肿。而使用 Maven 后每个 jar 包本身只在本地仓库中保存一份，需要 jar 包的工程只需要以坐标的方式简单的引用一下就可以了。不仅极大的节约了存储空间，让项目更轻巧，更避免了重复文件太多而造成的混乱。

- jar包需要别人替我们准备好或者到官网自行下载

​       JavaEE 开发中需要使用到的 jar 包种类繁多，几乎每个 jar 包在其本身的官网上的获取方式都不尽相同。为了查找一个 jar 包找遍互联网，身心俱疲，没有经历过的人或许体会不到这种折磨。不仅如此， 费劲心血找的 jar 包里有的时候并没有你需要的那个类，又或者又同名的类没有你要的方法——以不规范的方式获取的 jar 包也往往是不规范的。

​        使用 Maven 我们可以享受到一个完全统一规范的 jar 包管理体系。你只需要在你的项目中以坐标的方式依赖一个 jar 包，Maven 就会自动从中央仓库进行下载，并同时下载这个 jar 包所依赖的其他 jar 包——规范、完整、准确！一次性解决所有问题!

- 一个jar包依赖的其他jar包需要自己手动加入到项目中

​       jar 包往往不是孤立存在的，很多 jar 包都需要在其他 jar 包的支持下才能够正常工作，我们称之为 jar 包之间的依赖关系。最典型的例子是：commons-fileupload-1.3.jar 依赖于 commons-io-2.0.1.jar，如果没有 IO 包，FileUpload 包就不能正常工作。

​       而引入 Maven 后，Maven 就可以替我们自动的将当前 jar 包所依赖的其他所有 jar 包全部导入进来， 无需人工参与，节约了我们大量的时间和精力。用实际例子来说明就是：通过 Maven 导入 commons-fileupload-1.3.jar 后，commons-io-2.0.1.jar 会被自动导入，程序员不必了解这个依赖关系。

## What

### Maven简介

​       Maven 是 Apache 软件基金会组织维护的一款**自动化构建**工具，专注服务于 Java 平台的**项目构建**和**依赖管理**。

​       发展：Make—>Ant—>Maven—>Gradle

### 什么是构建

​       构建就是以我们编写的 Java 代码、框架配置文件、国际化等其他资源文件、JSP 页面和图片等静态资源作为“原材料”，去“生产”出一个可以运行的项目的过程。

- 编译：Java源文件(User.java)—>编译—>Class字节码文件(User.class)—>交给JVM执行
- 部署：一个BS项目最终运行的并不是动态Web工程本身，而是这个动态的Web工程“编译的结果”

### 构建过程的几个主要环节

- 清理：删除以前的编译结果，为重新编译做好准备。
- 编译：将 Java 源程序编译为字节码文件。
- 测试：针对项目中的关键点进行测试，确保项目在迭代开发过程中关键点的正确性。
- 报告：在每一次测试后以标准的格式记录和展示测试结果。
- 打包：将一个包含诸多文件的工程封装为一个压缩文件用于安装或部署。Java 工程对应 jar 包，Web 工程对应 war 包。
- 安装：在 Maven 环境下特指将打包的结果——jar 包或 war 包安装到本地仓库中。
- 部署：将打包的结果部署到远程仓库或将 war 包部署到服务器上运行。

### 自动化构建

​       可以自动的从构建过程的起点一直执行到终点：

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gvh7iv5s2yj615c06wjs002.jpg" style="zoom: 50%;" />

### Maven核心概念

- POM
- 约定的目录结构
- 坐标
- **依赖管理**
- 仓库管理
- 生命周期
- 插件和目标
- 继承
- 聚合

## How

​       Maven 的核心程序中仅仅定义了抽象的生命周期，而具体的操作则是由 Maven 的插件来完成的。可是Maven 的插件并不包含在 Maven 的核心程序中，在首次使用时需要联网下载。 下载得到的插件会被保存到本地仓库中。本地仓库默认的位置是：~\.m2\repository。

## 约定的目录结构

​       约定的目录结构对于 Maven 实现自动化构建而言是必不可少的一环，就拿自动编译来说，Maven 必须能找到 Java 源文件，下一步才能编译，而编译之后也必须有一个准确的位置保持编译得到的字节码文件。

​       我们在开发中如果需要让第三方工具或框架知道我们自己创建的资源在哪，那么基本上就是两种方式：

- 通过配置的形式明确告诉它
- 基于第三方工具或框架的约定

​       Maven 对工程目录结构的要求就属于后面的一种。

	Hello  //根目录（工程名）
	|---src  //源码
	|---|---main  //存放主程序
	|---|---|---java  //存放java源文件
	|---|---|---resources  //存放框架或其他工具的配置文件
	|---|---test  //存放测试程序
	|---|---|---java
	|---|---|---resources
	|---pom.xml  //Maven工程的核心配置文件

​       现在 JavaEE 开发领域普遍认同一个观点：**约定>配置>编码**。意思就是能用配置解决的问题就不编码， 能基于约定的就不进行配置。而 Maven 正是因为指定了特定文件保存的目录才能够对我们的 Java 工程进行自动化构建。

## Maven常用命令

	  mvn compile	编译主程序
	  mvn test-compile 编译测试程序
		mvn clean	清理
		mvn test	执行测试
		mvn package	打包
		mvn install 安装
		mvn site 生成站点
		※注意：运行Maven命令时一定要进入pom.xml文件所在的目录！

## POM

​       Project Object Model：项目对象模型。将 Java 工程的相关信息封装为对象作为便于操作和管理的模型。

​       pom.xml对于Maven 工程是核心配置。可以说学习 Maven 就是学习 pom.xml 文件中的配置。

## 坐标

### 几何中的坐标

- 在一个平面中使用 x、y 两个向量可以唯一的确定平面中的一个点。
- 在空间中使用 x、y、z 三个向量可以唯一的确定空间中的一个点。

### Maven 的坐标

​       使用如下三个向量在 Maven 的仓库中唯一的确定一个 Maven 工程： 

- groupid：公司或组织的域名倒序+当前项目名称

- artifactId：当前项目的模块名称

- version：当前模块的版本

  <groupId>com.atguigu.maven</groupId> 
  <artifactId>Hello</artifactId> 
  <version>0.0.1-SNAPSHOT</version>

### 如何通过坐标到仓库中查找jar包

- 将 gav 三个向量连起来

`com.atguigu.maven+Hello+0.0.1-SNAPSHOT` 

- 以连起来的字符串作为目录结构到仓库中查找

`com/atguigu/maven/Hello/0.0.1-SNAPSHOT/Hello-0.0.1-SNAPSHOT.jar`

**※注意：我们自己的 Maven 工程必须执行安装操作才会进入仓库。安装的命令是：mvn install**

## 依赖

### 依赖的目的

​       当 A jar 包用到了 B jar 包中的某些类时，A 就对 B 产生了依赖，这是概念上的描述。那么如何在项目中以依赖的方式引入一个我们需要的 jar 包呢?答案非常简单，就是使用 dependency 标签指定被依赖 jar 包的坐标就可以了。

```
<dependency>
    <groupId>com.atguigu.maven</groupId>
    <artifactId>Hello</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <scope>compile</scope>
</dependency>
```

### 依赖的范围

​       大家注意到上面的依赖信息中除了目标 jar 包的坐标还有一个 scope 设置，这是依赖的范围。依赖的范围有几个可选值，我们用得到的是：**compile、test、provided** 三个。

- 从项目结构角度理解 compile 和 test 的区别

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gvi6e4m0l7j60yw09yt9v02.jpg" style="zoom: 50%;" />

- 从开发和运行这两个不同阶段理解 compile 和 provided 的区别

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gvi6jdw00fj60wy0am76402.jpg" style="zoom:50%;" />

- 总结

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gvi6qwk1ymj618u05ugm802.jpg" style="zoom: 50%;" />

### 依赖的传递性

​       A 依赖 B，B 依赖 C，A 能否使用 C 呢？那要看 B 依赖 C 的范围是不是 compile，如果是则可用，否则不可用。

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gvi6sw9z7hj617005uaao02.jpg" style="zoom:50%;" />

### 依赖的排除

​       如果我们在当前工程中引入了一个依赖是 A，而 A 又依赖了 B，那么 Maven 会自动将 A 依赖的 B 引入当前工程，但是个别情况下 B 有可能是一个不稳定版，或对当前工程有不良影响。这时我们可以在引入 A 的时候将B排除。

- 例

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gvi6vpy1rwj615u08k0up02.jpg" style="zoom:50%;" />

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gvjhkqbicwj618u0iadic02.jpg" style="zoom: 33%;" />

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gvjhn1lonzj60zu07et9r02.jpg" style="zoom: 50%;" />

### 统一管理所依赖jar包的版本

​       对同一个框架的一组 jar 包最好使用相同的版本。为了方便升级框架，可以将 jar 包的版本信息统一提取出来：

- 统一声明版本号

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gvjhpvylflj618u04cdga02.jpg" style="zoom:50%;" />

​       其中 atguigu.spring.version 部分是自定义标签。

- 引用前面声明的版本号

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gvjhrkm6hwj618u0720tr02.jpg" style="zoom:50%;" />

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gvjhrxr9axj618u04c3ym02.jpg" style="zoom:50%;" />

- 其他用法

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gvjhswl32xj618u04aaak02.jpg" style="zoom:50%;" />

### 依赖的原则

​       **解决 jar 包冲突。**

- 路径最短者优先

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gvjhu2xkiwj611s09m0tu02.jpg" style="zoom:50%;" />

- 路径相同时先声明者优先

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gvjhzhs5ljj612a08caaz02.jpg" style="zoom:50%;" />

## 仓库

### 分类

（1）本地仓库：为当前本机电脑上的所有 Maven 工程服务。

（2）远程仓库：

- 私服：架设在当前局域网环境下，为当前局域网范围内的所有 Maven 工程服务。

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gvh96mhkfxj610c0bat9p02.jpg" style="zoom: 50%;" />

- 中央仓库：架设在 Internet 上，为全世界所有 Maven 工程服务。
- 中央仓库的镜像：架设在各个大洲，为中央仓库分担流量。减轻中央仓库的压力，同时更快的响应用户请求。

### 仓库中的文件

- Maven 的插件

- 我们自己开发的项目的模块

- 第三方框架或工具的 jar 包

  **※不管是什么样的 jar 包，在仓库中都是按照坐标生成目录结构，所以可以通过统一的方式查询或依赖。**

## 生命周期

### 什么是Maven的生命周期

- Maven 生命周期定义了各个构建环节的执行顺序，有了这个清单，Maven 就可以自动化的执行构建命令了。
- Maven 有三套相互独立的生命周期，分别是：

​       （1）Clean Lifecycle 在进行真正的构建之前进行一些清理工作。

​       （2）Default Lifecycle 构建的核心部分，编译，测试，打包，安装，部署等等。

​       （3）Site Lifecycle 生成项目报告，站点，发布站点。

​       它们是相互独立的，你可以仅仅调用 clean 来清理工作目录，仅仅调用 site 来生成站点。当然你也可以直接运行 mvn clean install site 运行所有这三套生命周期。

​       每套生命周期都由一组阶段(Phase)组成，我们平时在命令行输入的命令总会对应于一个特定的阶段。比如，运行 mvn clean，这个 clean 是 Clean 生命周期的一个阶段。有 Clean 生命周期，也有 clean 阶段。

### Clean 生命周期

​       Clean 生命周期一共包含了三个阶段：

- pre-clean 执行一些需要在 clean 之前完成的工作
- clean 移除所有上一次构建生成的文件
- post-clean 执行一些需要在 clean 之后立刻完成的工作

### Site生命周期

- pre-site 执行一些需要在生成站点文档之前完成的工作
- site 生成项目的站点文档
- post-site 执行一些需要在生成站点文档之后完成的工作，并且为部署做准备
- site-deploy 将生成的站点文档部署到特定的服务器上

​       这里经常用到的是 site 阶段和 site-deploy 阶段，用以生成和发布 Maven 站点，这可是 Maven 相当强大的功能，Manager 比较喜欢，文档及统计数据自动生成，很好看。

### Default 生命周期

​       Default 生命周期是 Maven 生命周期中最重要的一个，绝大部分工作都发生在这个生命周期中。这里，只解释一些比较重要和常用的阶段：

- validate
- generate-sources
- process-sources
- generate-resources
- process-resources 复制并处理资源文件，至目标目录，准备打包。
- compile 编译项目的源代码。
- process-classes
- generate-test-sources
- process-test-sources
- generate-test-resources
- process-test-resources 复制并处理资源文件，至目标测试目录。
- test-compile 编译测试源代码。
- process-test-classes
- test 使用合适的单元测试框架运行测试。这些测试代码不会被打包或部署。
- prepare-package
- package 接受编译好的代码，打包成可发布的格式，如 JAR。
- pre-integration-test
- integration-test
- post-integration-test
- verify
- install 将包安装至本地仓库，以让其它项目依赖。
- deploy 将最终的包复制到远程的仓库，以让其它开发人员与项目共享或部署到服务器上运行。

### 生命周期与自动化构建

​       **运行任何一个阶段的时候，它前面的所有阶段都会被运行**，例如我们运行 mvn install 的时候，代码会被编译，测试，打包。这就是 Maven 为什么能够自动执行构建过程的各个环节的原因。此外，Maven 的插件机制是完全依赖 Maven 的生命周期的，因此理解生命周期至关重要。

## 插件和目标

- Maven 的核心仅仅定义了抽象的生命周期，具体的任务都是交由插件完成的。

- 每个插件都能实现多个功能，每个功能就是一个插件目标。

- Maven 的生命周期与插件目标相互绑定，以完成某个具体的构建任务。

  例如：compile 就是插件 maven-compiler-plugin 的一个目标；pre-clean 是插件 maven-clean-plugin 的一个目标。

## 继承

### 为什么需要继承机制

​       由于非 compile 范围的依赖信息是不能在“依赖链”中传递的，所以有需要的工程只能单独配置。例如：

- Hello依赖的junit：4.0
- HelloFriend依赖的junit：4.0
- MakeFriend依赖的junit：4.9

​       由于**test的范围的依赖不能传递**，所以必然会分散在各个模块工程中，很容易造成版本不一致。此时如果项目需要将各个模块的 junit 版本统一为 4.9，那么到各个工程中手动修改无疑是非常不可取的。 使用继承机制就可以将这样的依赖信息统一提取到父工程模块中进行统一管理。

### 创建父工程

​       创建父工程和创建一般的 Java 工程操作一致，唯一需要注意的是：打包方式处要设置为 pom。

### 在子工程中引用父工程

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gvjra465k9j616y0bc76202.jpg" style="zoom:50%;" />

​        此时如果子工程的 groupId 和 version 如果和父工程重复则可以删除。

### 在父工程中管理依赖

​       将 Parent 项目中的 dependencies 标签，用 dependencyManagement 标签括起来：

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gvjrboi7q0j616w09wjsi02.jpg" style="zoom:50%;" />

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gvjrcn1mqnj616w04adg502.jpg" style="zoom:50%;" />

​        在子项目中重新指定需要的依赖，删除范围和版本号：

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gvjrebg18dj617008igmf02.jpg" style="zoom:50%;" />

## 聚合

### 为什么要使用聚合

​       将多个工程拆分为模块后，需要手动逐个安装到仓库后依赖才能够生效。修改源码后也需要逐个手动进行 clean 操作。而使用了聚合之后就可以批量进行 Maven 工程的安装、清理工作。

### 如何配置聚合

​       在总的聚合工程中使用 modules/module 标签组合，指定模块工程的相对路径即可：

<img src="https://tva1.sinaimg.cn/large/008i3skNly1gvjricecyqj616y0740tf02.jpg" style="zoom:50%;" />

## Maven 酷站

​       我们可以到 http://mvnrepository.com/搜索需要的 jar 包的依赖信息。