---
title: 安装JDK
date: 2023-01-29 16:08:21
tags: 
- Java
- Dev Tools
- SDK
---

# 本文章的一些特殊文法表述

为了保证您可以顺利使用这篇文章，此处先为您介绍一些本文可能用到的表述方式，您也可以选择先行阅读文章，遇到不确定的地方再回来查看其含义。

1. 对于所有的 **shell** 命令，本文将使用符号 `>` 表示一条 shell 指令的开始，通常在终端中显示为 `$` 或 `#` 等字符。
2. 对于所有的 **shell** 命令，文章内的变量均采用 `$$variable` 的格式。
3. 对于所有的 **shell** 命令，如果该内容是**根据情况可选**的（即选填变量），那么将会用 `[expression]` 进行表述。
4. 对于所有的 **shell** 命令，如果该内容是**根据情况调整数据**的，那么将会用 `<expression>` 进行表述。

# 确定您的平台

在进行操作前，您需要确认您所使用的平台。通常来说，平台由操作系统以及硬件架构组成，例如：

- windows-x86
- windows-x86_64

在您确定了您的平台之后，您就可以开始选择您需要或喜欢的 Java 构建及版本。

# 确定您需要使用的版本

**Java** 是一种广泛使用的电脑[编程语言](https://zh.wikipedia.org/wiki/编程语言)，擁有[跨平台](https://zh.wikipedia.org/wiki/跨平台)、[面向对象](https://zh.wikipedia.org/wiki/物件導向)、[泛型程序设计](https://zh.wikipedia.org/wiki/泛型程式設計)的特性，广泛应用于企业级Web应用开发和移动应用开发。 Java 语言自1995年诞生以来，受到了众多大型企业的欢迎。由于其开源的特性，任何企业与个人均可以对其进行修改，因此就产生了不同版本的 JDK 。总体来说，这些 JDK 均遵循 **OpenJDK** 标准。

## 现有的构建及版本

> 由于构建过多，此处只列举较为知名的构建；JDK 的版本通常分为两种：长期支持版 LTS 以及非长期支持版，具体版本信息可参照[Java Version History](https://en.wikipedia.org/wiki/Java_version_history)

- 由 Oracle 构建的 [Oracle JDK](https://www.oracle.com/java/) 
- 由 Microsoft 构建的 [Microsoft Build of OpenJDK](https://www.microsoft.com/openjdk)
- 由 Amazon 构建的 [Amazon Corretto](https://aws.amazon.com/corretto)
- 由 Eclipse 基金会构建的 [Eclipse Temurin](https://projects.eclipse.org/projects/adoptium.temurin)
- 由 Azul 构建的 [Zulu JDK](https://www.azul.com/downloads/?package=jdk)

> 除上述提到的构建之外，还有许多公司可能拥有其自行构建的可能是开源的基于 OpenJDK 进行修改的 JDK 版本，若您需要特定公司的开源版本的 OpenJDK ，您可以通过自行搜索关键词：<公司名称> + JDK，例如“腾讯公司 JDK”。

在完成构建选择后，请您自行点击对应的链接进入其下载页面。

# 安装及配置

## 安装

### Windows 或 macOS 用户

许多公司/组织都提供其构建版本的 Windows 或 macOS 安装程序，您只需要运行其提供的安装程序即可成功在您的计算机中安装 JDK。

### Linux 用户

部分公司/组织为特定的一部分 Linux 系统（如 Ubuntu 、 CentOS 等）提供了安装包，但是其余的并没有可以直接执行的安装包。此时您可以选择以 `.tar.gz` 或 `.tar.xz` 为后缀的文件进行下载，通常这是该公司/组织提供的二进制可执行程序包，解压即可运行。在完成下载后，您可以直接解压这个压缩包以获得完整的二进制程序包。

## 配置

在完成了上述安装步骤之后，我们需要将 Java 程序的安装路径设置到系统的环境变量中，通常这个变量以 `JAVA_HOME` 命名。

### Linux

根据 Linux 官方推荐，创建文件 `/etc/profile.d/java.sh` 并填写以下内容。

```shell
export JAVA_HOME=<$$path_to_javahome>
export PATH=$PATH:$JAVA_HOME/bin
```

### macOS ( >= 10.15.x )

根据 Apple 对系统的设计，您需要向文件 `/etc/zshrc` 中添加如下内容：

```shell
export JAVA_HOME=$(/usr/libexec/java_home -v <$$java_version>)
```

再创建文件 `/etc/paths.d/java` 并添加如下内容：

```shell
$JAVA_HOME/bin
```

### Windows

您需要进行以下步骤以完成 `JAVA_HOME` 的配置。

> 以下内容图片均以 Windows 11 系统作为展示。

1. 使用鼠标右击**此电脑**，并在弹出的画面中点击**属性**选项。
   ![image-20230129170845823](https://dist.cq.vorbote.cn/images/typora-images/image-20230129170845823.png)
2. 在弹出的界面中点击**高级系统设置**。
   ![image-20230129171127239](https://dist.cq.vorbote.cn/images/typora-images/image-20230129171127239.png)
3. 在弹出的界面中点击**环境变量设置**。
   ![image-20230129172304833](https://dist.cq.vorbote.cn/images/typora-images/image-20230129172304833.png)
4. 在弹出的界面中点击任意一个**新增**。（上面的表格是**用户环境变量**，下面的表格是**系统环境变量**，如果需要让所有人访问则需要添加在**系统环境变量**）
   ![image-20230129172527895](https://dist.cq.vorbote.cn/images/typora-images/image-20230129172527895.png)
5. 在弹出的界面中输入对应的**变量名**及**变量值**。完成填写后点击**确认**以保存设置。
   ![image-20230129172935189](https://dist.cq.vorbote.cn/images/typora-images/image-20230129172935189.png)

6. 双击 `Path` ，请注意：一定要在第4步中点击的那一栏中寻找 `Path` ，并将如下文本添加到最末尾。
   ```shell
   %JAVA_HOME%\bin
   ```

# 验证安装

打开系统中的终端（Linux 或 macOS）或 PowerShell （Windows），并运行如下指令：

```shell
java -version
```

如果有类似如下格式的输出，则为安装成功。

```shell
openjdk version "17.0.3" 2022-04-19 LTS
OpenJDK Runtime Environment Corretto-17.0.3.6.1 (build 17.0.3+6-LTS)
OpenJDK 64-Bit Server VM Corretto-17.0.3.6.1 (build 17.0.3+6-LTS, mixed mode, sharing)
```

