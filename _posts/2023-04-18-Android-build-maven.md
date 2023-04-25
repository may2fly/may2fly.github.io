---
layout: post
title: Maven
subtitle: Android构建工具Maven介绍
categories: Android
tags: [Android]
---

Maven是一个基于Java的项目管理和构建自动化工具。它的主要作用是管理项目依赖、构建项目、运行测试和部署项目。Maven使用XML文件来配置项目，并通过中央仓库和其他远程仓库来获取依赖项。

Maven的构建过程主要包括以下几个阶段：

1、清理 (clean)：删除之前构建产生的所有文件。

2、编译 (compile)：编译项目源代码。

3、测试 (test)：运行单元测试。

4、打包 (package)：将项目打包成可执行的JAR或WAR文件。

5、集成测试 (integration-test)：运行集成测试。

6、部署 (deploy)：将构建产物部署到远程仓库或服务器上。

Maven的核心概念，包括：

* POM (Project Object Model)：Maven使用POM文件来描述项目的基本信息和依赖关系。POM文件位于项目根目录下的pom.xml文件中。
* 仓库 (Repository)：Maven使用仓库来存储项目依赖和构建产物。Maven有两种类型的仓库：本地仓库和远程仓库。本地仓库是指存储在本地机器上的依赖和构建产物，而远程仓库则是指存储在远程服务器上的依赖和构建产物。
* 插件 (Plugin)：Maven使用插件来扩展其功能。Maven有很多内置插件，也可以通过自定义插件来满足特定的需求。

# 插件与版本
Maven有大量的插件和版本可供选择，这些插件和版本可以用于构建，测试和部署Java应用程序。`kotlin-maven-plugin` 用于编译`Kotlin`源代码与模块，目前只支持 Maven V3。

通过`kotlin.version` 属性定义要使用的 Kotlin 版本:

```xml
<properties>
<kotlin.version>{{ site.data.releases.latest.version }}</kotlin.version>
</properties>
```

# 依赖

Kotlin 有一个广泛的标准库可用于应用程序。 To use the standard library in your project, 在 pom 文件中配置以下依赖关系:

```xml
<dependencies>
    <dependency>
        <groupId>org.jetbrains.kotlin</groupId>
        <artifactId>kotlin-stdlib</artifactId>
        <version>${kotlin.version}</version>
    </dependency>
</dependencies>
```

如果项目使用 Kotlin 反射或者测试设施，那么你还需要添加相应的依赖项。 其构件 ID 对于反射库是 kotlin-reflect ，对于测试库是 kotlin-test 与 kotlin-test-junit 。

# 编译只有 Kotlin 的源代码

要编译源代码，请在 <build> 标签中指定源代码目录:

```xml
<build>
    <sourceDirectory>${project.basedir}/src/main/kotlin</sourceDirectory>
    <testSourceDirectory>${project.basedir}/src/test/kotlin</testSourceDirectory> 
</build>
```

需要引用 Kotlin Maven 插件来编译源代码:

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.jetbrains.kotlin</groupId>
            <artifactId>kotlin-maven-plugin</artifactId>
            <version>${kotlin.version}</version>
            <executions>
                <execution>
                    <id>compile</id>
                    <goals>
                        <goal>compile</goal>
                    </goals>
                </execution>
                <execution>
                    <id>test-compile</id>
                    <goals>
                        <goal>test-compile</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```

# 同时编译 Kotlin 与 Java 源代码
要在 Maven 中编译混合代码应用程序，需要确保 Kotlin 编译器在 Java 编译器之前被调用。这可以通过在 Maven 构建中正确配置 kotlin-maven-plugin 和 maven-compiler-plugin 来实现。

为了确保 kotlin-maven-plugin 在 maven-compiler-plugin 之前运行，您需要将其配置添加到 pom.xml 文件中，并将其放置在 maven-compiler-plugin 配置之前。

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.jetbrains.kotlin</groupId>
            <artifactId>kotlin-maven-plugin</artifactId>
            <version>${kotlin.version}</version>

            <executions>
                <execution>
                    <id>compile</id>
                    <goals>
                    <goal>compile</goal>
                </goals>
                <configuration>
                    <sourceDirs>
                        <sourceDir>${project.basedir}/src/main/kotlin</sourceDir>
                        <sourceDir>${project.basedir}/src/main/java</sourceDir>
                    </sourceDirs>
                </configuration>
            </execution>

            <execution>
                <id>test-compile</id>
                <goals> <goal>test-compile</goal> </goals>
                <configuration>
                    <sourceDirs>
                        <sourceDir>${project.basedir}/src/test/kotlin</sourceDir>
                        <sourceDir>${project.basedir}/src/test/java</sourceDir>
                    </sourceDirs>
                </configuration>
            </execution>
        </executions>
    </plugin>

    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.5.1</version>
        <executions>
            <!-- 替换会被 maven 特别处理的 default-compile --> 
            <execution>
                <id>default-compile</id>
                <phase>none</phase>
            </execution>
                <!-- 替换会被 maven 特别处理的 default-testCompile --> 
                <execution>
                <id>default-testCompile</id>
                <phase>none</phase>
            </execution>
            <execution>
                <id>java-compile</id>
                <phase>compile</phase>
                <goals>
                    <goal>compile</goal>
                </goals>
            </execution>
            <execution>
                <id>java-test-compile</id>
                <phase>test-compile</phase>
                <goals>
                    <goal>testCompile</goal>
                </goals>
            </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```

# 增量编译
为了使构建更快，可以为 Maven 启用增量编译(从 Kotlin 1.1.2 起支持)。 为了做到这一点，需要定义 kotlin.compiler.incremental 属性:

```xml
<properties> 
    <kotlin.compiler.incremental>true</kotlin.compiler.incremental>
</properties>
```
或者，使用 -Dkotlin.compiler.incremental=true 选项运行构建。

# 注解处理

# Jar 文件
要创建一个仅包含模块代码的小型Jar文件，请在Mavenpom.xml文件中的 build->plugins下面包含以下内容， 其中 main.class 定义为一个属性，并指向主 Kotlin 或 Java 类:

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-jar-plugin</artifactId>
    <version>2.6</version>
    <configuration>
        <archive>
            <manifest>
                <addClasspath>true</addClasspath>
                <mainClass>${main.class}</mainClass>
            </manifest>
        </archive>
    </configuration>
</plugin>
```

# 独立的 Jar 文件
要创建一个独立的(self-contained)Jar 文件，包含模块中的代码及其依赖项，请在 Mavenpom.xml 文件中的 build->plugins 下面包含以下内容其中 指向主 Kotlin 或 Java 类:

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-assembly-plugin</artifactId>
    <version>2.6</version>
    <executions>
        <execution>
            <id>make-assembly</id>
            <phase>package</phase>
            <goals> <goal>single</goal> </goals>
            <configuration>
                <archive>
                    <manifest>
                        <mainClass>${main.class}</mainClass>
                    </manifest>
                </archive>
                <descriptorRefs>
                    <descriptorRef>jar-with-dependencies</descriptorRef>
                </descriptorRefs>
            </configuration>
        </execution>
    </executions>
</plugin>
```
这个独立的 jar 文件可以直接传给 JRE 来运行应用程序:


# 指定编译器选项
可以将额外的编译器选项与参数指定为 Maven 插件节点的<configuration> 元素下的标签 :

```xml
<plugin>
    <groupId>org.jetbrains.kotlin</groupId>
    <artifactId>kotlin-maven-plugin</artifactId>
    <version>${kotlin.version}</version>
    <executions>......</executions>
    <configuration>
        <nowarn>true</nowarn> <!-- 禁用警告 -->
        <args>
            <arg>-Xjsr305=strict</arg> <!-- 对 JSR-305 注解启用严格模式 -->
            ...
        </args>
    </configuration>
</plugin>
```

许多选项还可以通过属性来配置:

```xml
<project ......>
    <properties>
<kotlin.compiler.languageVersion>1.0</kotlin.compiler.languageVersion> </properties>
</project>
```

# 生成文档
标准的 Javadoc 生成插件( maven-javadoc-plugin )不支持 Kotlin 代码。 要生成 Kotlin 项目 的文档，请使用 Dokka; 相关配置说明请参见 Dokka README 。Dokka 支持混合语言项目， 并且可以生成多种格式的输出 ，包括标准 Javadoc。