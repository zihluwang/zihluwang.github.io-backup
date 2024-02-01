---
title: 安装 Gradle
date: 2024-01-31 19:07:00
tags: [Java, Build Tools, Gradle]
---

# 安装 Gradle

> 本文使用的 Gradle 版本为 8.5

1. 确保你的电脑已经安装并配置好 Java （version &ge; 8）。
2. 从 [Gradle 官方网站 - Releases 页面](https://gradle.org/releases/)选择对应的版本进行下载。

   官网如下图所示，选择 **binary-only** 和 **complete** 皆可。

   ![Gradle Official Site Screenshot](https://dist.cq.vorbote.cn/images/install-gradle-01-gradle-official-site.png)

3. 下载完成后，将 **.zip** 压缩包解压缩后放在任意位置（本文以 `/path/to/gradle` 指代 `gradle` 解压后的目录）。

   解压后的目录有如下文件：

   ![Unzipped Gradle Directory](https://dist.cq.vorbote.cn/images/install-gradle-02-unzipped-gradle-directory.png)

4. 配置 `GRADLE_HOME` 与 `GRADLE_USER_HOME` 目录。

   `GRADLE_HOME` 是 Gradle 的安装目录，而 `GRADLE_USER_HOME` 则是用于存放 Gradle 所需的文件的路径，本文以 Gradle 的默认路径，即 `~/.gradle`，**此路径也是不配置 `GRADLE_USER_HOME` 系统环境变量时的默认值**。

# 【可选】对 Gradle 进行配置

如果需要对 Gradle 进行配置，可以通过初始化脚本的方式。Gradle 会按照如下优先级对项目进行配置：

1. 通过命令行的 `-I` 或 `--init-script` 指定所使用的初始化脚本。
2. 处在 Gradle User Home 中的 `init.gradle` 或 `init.gradle.kts` 文件。
3. 处在 Gradle User Home 中的 `init.d` 文件夹中的以 `.gradle` 或 `.init.gradle.kts` 结尾的文件。
4. 处在 Gradle Home 中的 `init.d` 文件夹中的以 `.gradle` 或 `.init.gradle.kts` 结尾的文件。

> 因为 Gradle 目前推荐使用 Kotlin 作为其 DSL 语言，因此本文所使用的均为 Kotlin 脚本。

## 配置文件的格式

理论上来说配置文件需要影响所有使用 Gradle 作为构建工具的项目，如需要创建对所有项目生效的配置，文件的格式应该如下所示。

```kotlin
allprojects {
    // 你的自定义配置
}
```

## 配置 Gradle 的依赖存储库

你可以通过以下方式设置 Gradle 所使用的存储库。

```kotlin
repositories {
    mavenLocal() // 使用 Maven 本地仓库
    maven(url = "repo_url") // 使用指定路径的 Maven 远程仓库
    mavenCentral() // 使用 Maven 中央仓库
    google() // 使用 Maven Google 仓库
}
```

