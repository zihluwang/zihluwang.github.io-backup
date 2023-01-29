---
title: 安装 Maven
date: 2023-01-30 02:27:46
tags:
- Java
- Dev Tools
- Build Tools
---

Apache Maven ，是一个软件（特别是 Java 软件）项目管理及自动构建工具，由 Apache 软件基金会所提供。 Maven 也可被用于构建和管理各种项目，例如 C# ， Ruby ， Scala 和其他语言编写的项目。 Maven 曾是 Jakarta 项目的子项目，现为由 Apache 软件基金会主持的独立 Apache 项目。

Maven 解决了软件构建的两方面问题：一是软件是如何构建的，二是软件的依赖关系。不同于 Apache Ant 等早期工具， Maven 设定了构建流程的标准，在此之外只需要指定例外情况。XML文件描述了正在构建的软件项目、它对其他外部模块和组件的依赖关系、构建顺序、目录和所需的插件。该文件通常有预设的目标任务，例如代码编译和打包。Maven从一个或多个代码仓库（例如Maven 2 Central Repository）动态地下载Java库与Maven插件，并将其存储在本地缓存区中<sup>[[1]](https://zh.wikipedia.org/wiki/Apache_Maven)</sup>。

# 本文章的一些特殊文法表述

为了保证您可以顺利使用这篇文章，此处先为您介绍一些本文可能用到的表述方式，您也可以选择先行阅读文章，遇到不确定的地方再回来查看其含义。

1. 对于所有的交互型 **shell** 命令，本文将使用符号 `>` 表示一条 shell 指令的开始，通常在终端中显示为 `$` 或 `#` 等字符。
2. 对于所有的 **shell** 命令，文章内的变量均采用 `$$variable` 的格式。
3. 对于所有的 **shell** 命令，如果该内容是**根据情况可选**的（即选填变量），那么将会用 `[expression]` 进行表述。
4. 对于所有的 **shell** 命令，如果该内容是**根据情况调整数据**的，那么将会用 `<expression>` 进行表述。

# 先决条件

Apache Maven 最新版对于系统的要求如下：

- JDK：需要在 1.7 版本以上；
- 磁盘空间：需要约10 MiB 的空间安装 Maven 工具链，本地存储库的空间大小会随时变化，但是期望至少拥有 500 MiB 的磁盘空间。

# 下载 Apache Maven

[点击这里](https://maven.apache.org/download.cgi)跳转至 Apache Maven 下载页面，页面如下图所示。 Windows 用户请下载 **Binary zip archive** 文件， Linux / macOS 用户请下载 **Binary tar.gz archive** 文件。

![image-20230130024314247](https://dist.cq.vorbote.cn/image/png/lRUEbJ-1675017794.png)

# 安装 Apache Maven 并配置

选择任意一款您喜欢的解压缩工具将 Apache Maven 解压缩，并按照如下文档进行配置。

## 针对 Linux 用户

根据 Linux 官方推荐，请创建文件 `/etc/profile.d/maven.sh` 并在其中填写如下内容：

```shell
export M2_HOME=<$$path_to_maven_homedir>
export PATH=$PATH:$M2_HOME/bin
```

## 针对 macOS 用户

根据 Apple 对 macOS 操作系统的设计，你需要首先前往 `/etc/zshrc` 中添加如下内容：

```shell
export M2_HOME=<$$path_to_maven_homedir>
```

接下来创建文件 `/etc/paths.d/maven` 并向其中添加如下内容：

```shell
$M2_HOME/bin
```

## 针对 Windows 用户

> 该操作容易出现问题，请先在 PowerShell 或 Command Prompt 中运行 `echo %Path%` ，并将输出内容保存好。

首先请使用管理员身份运行 **PowerShell** 或 **Command Prompt** ，并运行如下指令：

```powershell
> setx /m M2_HOME <$$path_to_maven_homedir>
> setx /m Path "%Path%;%M2_HOME%\bin"
```

或者您也可以使用图形化的方式进行操作。

# 验证安装

完成安装后，在终端或 PowerShell 内运行指令 `mvn -v` ，并看到如下格式的输出，则表示安装成功。

```shell
Apache Maven $$some_version ($$maven_checksum)
Maven home: $$path_to_maven_homedir
Java version: $$your_java_version, vendor: $$java_builder, runtime: $$java_home
Default locale: $$your_system_locale, platform encoding: $$your_platform_encoding
OS name: "$$os_name", version: "$$os_version", arch: "$$os_arch", family: "$$os_family"
```

# 额外配置

除安装到系统中的配置外，本文额外讲述一部分 Maven 配置文件 `<path_to_maven_home>/conf/settings.xml` 中的部分常被修改的配置项。

> 由于其配置文件采用 XML 格式，故此处采用树形结构以更好地表达节点的概念。

- `settings` - 文件的根节点。
  - `localRepository` - 本地存储库路径。
  - `mirrors` - 镜像服务器地址。
    - `mirror` - 单个镜像配置。
      - `id` - 这个镜像的唯一 ID。
      - `mirrorOf` - 指定用于镜像哪个存储库。
      - `name` - 人类可阅读的镜像名称。
      - `url` - 访问镜像的 URL 。

更多设置请参阅后续[进阶设置](javascript:alert('客观先别急，这个玩意儿暂时还没写出来~'))。

# 中国大陆镜像地址推荐

- 中国科学技术大学镜像地址：https://maven.proxy.ustclug.org/maven2 （墙裂推荐作为**中央仓库**的镜像地址）

- 阿里云 Maven 镜像

  - Central 仓库：https://maven.aliyun.com/repository/central
  - jCenter 仓库镜像：https://maven.aliyun.com/repository/public
  - Central 及 jCenter 聚合仓库镜像：https://maven.aliyun.com/repository/public

  - 更多阿里云镜像地址请参阅[仓库服务](https://developer.aliyun.com/mvn/guide)

