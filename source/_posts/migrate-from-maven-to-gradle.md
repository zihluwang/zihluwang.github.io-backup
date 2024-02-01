---
title: 将 Java 类库由 Maven 转换为 Gradle
date: 2024-02-01 12:48:03
tags:
  - Java
  - Build Tools
  - Maven
  - Gradle
---
# 将 Java 类库由 Maven 转换为 Gradle

> Gradle 版本为 Gradle 8.5

## 创建 `settings.gradle.kts` 文件及 `build.gradle.kts` 文件

在项目根目录下创建 `settings.gradle.kts` 及 `build.gradle.kts` 文件，并在 settings.gradle.kts 文件中对项目进行创建。

### `settings.gradle.kts`

```kotlin
rootProject.name = "project_name"

// 如果你的项目包含了多个模块，请使用下面的脚本。
include("path/to/sub-module", ...)
```

### `build.gradle.kts`

你需要在这个文件中定义项目的构建情况，例如使用的插件、使用的依赖等等。在此时你可以仅创建该文件，**无需在里面声明构建脚本**。

## 下载 Gradle Wrapper

在项目根目录使用命令 `gradle wrapper` 下载 **Gradle Wrapper**。由于 Gradle 每个版本的 API 都可能有重大变化，因此强烈推荐使用 Gradle Wrapper 来作为构建工具。直接使用该指令将会默认使用您本地安装的 Gradle 版本作为 Wrapper。【可选】如果您对其使用的默认版本不满意，也可以再次使用命令 `gradle wrapper --gradle-version <一个 Gradle 版本号>` 来安装指定版本的 Gradle Wrapper。

## 在 Gradle User Home 下创建 gradle.properties 文件

