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

> Gradle 版本为 Gradle 8.5。
>
> 本文所使用的示例代码均可在 [GitHub](https://github.com/zihluwang/migrate-to-gradle-sample) 上查看

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

该文件的主要目的是配置无法公布到公共代码库的一些机密信息，例如由 Sonatype 代管的 Maven Central Nexus 服务器的用户名和密码等等。你需要使用 `key=value` 的格式声明你的用户名和密码。

> 此处我们假定您需要将构建出的文件发布到 Maven Central 库。

一份示例 `gradle.properties` 文件如下：

```properti
repo.ossrh.host=https://s01.oss.sonatype.org/service/local/staging/deploy/maven2/
repo.ossrh.username=sample_user
repo.ossrh.password=sample_passcode
```

此处使用了带有 `.` 形式的 key，你也可以根据自己的情况选择使用驼峰式的 key。

如果您需要将您的 artifact 签名，请在此文件内添加如下 key：

- `signing.keyId`：GPG 密钥 ID 的后 8 位
- `signing.password`：您密钥的访问密码
- `signing.secretKeyRingFile`：GPG 私钥的路径，可以使用 `gpg --export-secret-keys -o <path/to/secretKeyRingFile>` 导出。

## 配置构建脚本

### 配置插件

Gradle 是一款依赖于 Task 的构建工具，使用插件可以将一些预定义的 Task 添加至您的项目中。对于一个 Java 项目，可以使用如下的代码来声明使用 Java 插件。

```kotlin
plugins {
  id("java")
  // 或
  java
}
```

如果您还需要一些其他预定义的 Task 来执行您的构建，请前往 [Gradle Plugins 官网](https://plugins.gradle.org)查询。

如果您开发的是 Java 类库，可以直接使用以下的配置。

```kotlin
plugins {
  id("java")
  id("java-library")
  id("maven-publish")
  id("signing")
}
```

### 配置 Java 版本以及是否要构建 javadoc 及 sources 包

使用以下指令配置您项目的 Java 版本。

```kotlin
java {
    sourceCompatibility = JavaVersion.VERSION_17
    targetCompatibility = JavaVersion.VERSION_17
  	withJavadocJar() // 使用此命令构建 Javadoc 包
    withSourcesJar() // 使用此命令构建 sources 包
}
```

### 【可选】配置项目的编译时的字符集

```kotlin
tasks.withType<JavaCompile> {
    options.encoding = "UTF-8"
}
```

### 【可选】声明 Maven 远程仓库

```kotlin
repositories {
  mavenLocal()
  maven(url = "https://codecrafters.coding.net/public-artifacts/base/public/packages/")
  maven(url = "https://maven.proxy.ustclug.org.cn/maven2/")
  mavenCentral()
}
```

### 配置依赖

#### 如何配置依赖

在 Gradle 中，虽仍然采用了 Maven 中的 gav 定位法，但是格式产生了很大的变化。首先，在 Gradle 的配置中，gav 可以直接采用 `"groupId:artifactId:version"` 的方式声明，例如

```xml
<dependency>
	<groupId>org.projectlombok</groupId>
  <artifactId>lombok</artifactId>
  <version>1.18.30</version>
  <scope>compile</scope>
</dependency>
```

```kotlin
dependencies {
  compileOnly("org.projectlombok:lombok:1.18.30")
}
```

#### 理解 Gradle 依赖中的各种 scope

>  请参考 [Maven Scope and Gradle Configurations](https://reflectoring.io/maven-scopes-gradle-configurations)、[Maven scope 与 Gradle scope - by herionZhang@CSDN](https://blog.csdn.net/zh452647457/article/details/108772045)、

#### 【可选】对依赖关系进行配置

```kotlin
configurations {
  compileOnly {
    extendsFrom(annotationProcessor.get())
  }
}
```

### 【可选】配置发布脚本

```kotlin
publishing {
    publications {
        create<MavenPublication>("<name_of_this_publication>") {
            groupId = "<your group id>"
            artifactId = "<your artifact id>"
            version = "<your version>"

            pom {
                // 定义生成的 POM 文件
                name = "<artifact name>"
                description = "<artifact description>"
                url = "<artifact home page>"

                licenses {
                    license {
                        name = "<licence name>"
                        url = "<license link>"
                    }
                }
                
                scm {
                    connection = "<scm connection>"
                    developerConnection = "<developer connection>"
                    url = "project source code url"
                }

                developers {
                    developer {
                        id = "<developer id>"
                        name = "<developer name>"
                        email = "<developer's email>"
                        timezone = "<developer's timezone>"
                    }
                }
            }

            from(components["java"]) // 这一行表示需要发布 jar 包

            // 如果不需要签名可以把这一段去掉
            signing {
                sign(publishing.publications["<name_of_this_publication>"])
            }
        }

        repositories {
            // 在这里配置你的目标仓库，可以有多个
            maven { // 这是 Maven 中央仓库的示例
                name = "sonatypeNexus"
                url = URI(providers.gradleProperty("repo.ossrh.host").get())
                credentials {
                    username = providers.gradleProperty("repo.ossrh.username").get()
                    password = providers.gradleProperty("repo.ossrh.password").get()
                }
            }
        }
    }
}
```

## 【可选】删除 pom.xml 文件

如果您的库不准备继续使用 Maven 作为构建工具，此时您就可以大胆放心的删除 `pom.xml` 文件了！
