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
export M2_HOME=
```

