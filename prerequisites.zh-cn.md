# 先决条件

在编写任何代码之前, 请安装最新的 JDK 8, Apache Maven, 以及您喜欢的 IDE.

## Java 8

建议使用 Oracle Java 8. 对于 Windows 用户, 只需要到 [Oracle Java 网站](http://java.oracle.com)下载并安装到您的系统. Redhat 刚刚在 DevNation 2016 上发布了 OpenJDK 8 给 Windows 用户 , 如果你坚持使用 OpenJDK, 请去 [Redhat Developers 网站](https://developers.redhat.com) 获取.
	
大多数 Linux 发行版都包含 OpenJDK, 通过 Linux 软件包管理器进行安装.

可选, 您可以设置 **JAVA\_HOME** 环境变量,并在 **PATH** 环境变量中添加 *&lt;JDK 安装目录>/bin*.

在系统终端中键入此命令以验证您的 Java 环境是否正确安装.

```
#java -version
java version "1.8.0_102"
Java(TM) SE Runtime Environment (build 1.8.0_102-b14)
Java HotSpot(TM) 64-Bit Server VM (build 25.102-b14, mixed mode)
```

## Apache Maven
   
下载最新的 Apache Maven 版本 [http://maven.apache.org](http://maven.apache.org), 在本地系统中解压缩它. 

可选, 你可以设置 **M2\_HOME** 环境变量, 也不要忘记追加 *&lt;Maven 安装目录>/bin* 去您的 **PATH** 环境变量.  

输入以下命令来验证 Apache Maven 正在工作.

```
#mvn -v
Apache Maven 3.3.9 (bb52d8502b132ec0a5a3f4c09453c07478323dc5; 2015-11-11T00:41:47+08:00)
Maven home: D:\build\maven
Java version: 1.8.0_102, vendor: Oracle Corporation
Java home: D:\jdk8\jre
Default locale: en_US, platform encoding: Cp1252
OS name: "windows 10", version: "10.0", arch: "amd64", family: "dos"
```	
	
如果你是 Gradle 的粉丝, 你可以使用 Gradle 作为构建工具. Gradle 可以成为 Apache Maven 的替代品.

## Lombok

我想用 Lombok 来简单地编码,并使代码清洁. 请去[Lombok project](https://projectlombok.org/)了解 Lombok.

baeldung 的博客有一份很好的 [Lombok 介绍](http://www.baeldung.com/intro-to-project-lombok).

## IDE 

源代码是基于 Maven, 独立于 IDE, 所以你可以选择你喜欢的 IDE. 目前流行的 IDEs 包括 Eclipse, IDEA, NetBeans.

我们将使用 JPA 标准元数据提供类型安全的查询,并使用 Lombok 来简化代码, 您必须在您的 IDEs 中启用 **Annotation Processing** 功能. 

### Spring Tool Suite

Spring Tool Suite 是一个基于 Eclipse 的 IDE, 并提供了很多内置的 Spring 支持,强烈建议新的 Spring 用户使用.

跳转至 [Spring 官网站点](https://spring.io),下载一份[Spring Tool Suite](https://spring.io/tools/sts) 的副本. 目前最新的版本是 3.8.

或者,您可以从 [Eclise official website](https://www.eclipse.org) 下载 Eclipse Java EE bu软件包的副本 , 并从 Eclipse 市场 安装 STS 插件.
	
将文件解压缩到本地磁盘. 进入根文件夹,有 **STS.exe** 文件, 双击它并启动 Spring Tool Suite.

1. 点击 Windows/Preference 菜单, 打开 Preference 对话框
2. 搜索 `Annotation`...
3. 展开 *Compiler/ Annotation Processing* , 允许 **Annotation Processing**.
4. 展开 *Maven/Annotation Processing*, 允许 **Annotation Processing**. 如果不存在该选项, 首先在 **Maven/Discovery** 安装 **m2e-apt**.  
5. 应用所有更改.

跳转 Lombok 项目网站, 并按照官方 [安装指南](https://projectlombok.org/setup/eclipse)) 将 Lombok 插件安装到 Eclipse IDE 中.

### Intellij IDEA	

毫无疑问, [Intellij IDEA](https://www.jetbrains.com/idea) 是最有生产力的 Java IDE. 它包括免费的开源社区版本和企业版本.

1. 点击 File / Settings 
2. 搜索 `annotation processor`
3. 允许 Annotation processing

You can install Lombok plugin from IDEA plugin manager to get Lombok support in your IDEA.

1. 点击 File / Settings / Plugins
2. 点击 Browse repositories...
3. 搜索 `Lombok Plugin`
4. 点击 Install plugin
5. 重启 IDE 

### NetBeans

[NetBeans](http://www.netbeans.org)是 Java 开发最简单的 IDE, 最初由 Sun (后来由 Oracle 维护) 提供, 它是免费且开源的.

现在 Oracle 把它称为 [Apache 基金会下的一个孵化器项目](http://netbeans.apache.org).

从 [netbeans 网站](https://netbeans.org) 下载 NetBeans 的副本(在 Apache 移交之前它仍然在运行).

NetBeans 用户, 不需要设置 Annotation Processing 以及 Lombok, NetBeans 默认情况下已经激活 Annotation processing 功能.

在接下来的文章中, 让我们尝试为我们的博客示例应用程序创建一个项目框架.