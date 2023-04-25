---
layout: post
title: Ant
subtitle: Android构建工具Ant介绍
categories: Android
---

Ant（Apache Ant）是一个开源的构建工具，用于自动化构建、测试和部署Java应用程序。Ant是一种基于XML的构建工具，使用简单，灵活性高，并且可以与其他工具无缝集成。

Ant通过定义一系列任务（Task）和目标（Target）来构建和管理项目。开发人员可以使用Ant来编译Java代码、运行单元测试、打包、部署应用程序等等。

# 获取 Ant 任务
Kotlin 为 Ant 提供了三个任务:

* kotlinc :面向JVM的Kotlin编译器
* kotlin2js :面向JavaScript的Kotlin编译器
* withKotlin :使用标准javacAnt任务时编译Kotlin文件的任务

这仨任务在 kotlin-ant.jar 库中定义，该库位于 Kotlin 编译器包中的 1.8.2+ 版本。

# 面向 JVM 只用 Kotlin 源代码
当项目由 Kotlin 专用源代码组成时，编译项目的最简单方法是使用`kotlinc`任务:

```xml
<project name="Ant Task Test" default="build">
    <typedef resource="org/jetbrains/kotlin/ant/antlib.xml" classpath="${kotlin.lib}/kotlin-ant.jar"/>

    <target name="build">
        <kotlinc src="hello.kt" output="hello.jar"/>
    </target>
</project>
```
其中 `${kotlin.lib}` 指向解压缩 Kotlin 独立编译器所在文件夹。

# 面向 JVM 只用 Kotlin 源代码且多根
如果项目由多个源代码根组成，那么使用 src 作为元素来定义路径:

```xml
<project name="Ant Task Test" default="build">
    <typedef resource="org/jetbrains/kotlin/ant/antlib.xml" classpath="${kotlin.lib}/kotlin-ant.jar"/>
    <target name="build">
        <kotlinc output="hello.jar">
            <src path="root1"/>
            <src path="root2"/>
        </kotlinc>
    </target>
</project>
```

# 面向 JVM 使用 Kotlin 和 Java 源代码
如果项目由 Kotlin 和 Java 源代码组成，虽然可以使用 `kotlinc` 来避免任务参数的重复，但是建议使用 `withKotlin` 任务:

```xml
<project name="Ant Task Test" default="build">
    <typedef resource="org/jetbrains/kotlin/ant/antlib.xml" classpath="${kotlin.lib}/kotlin-ant.jar"/>
    
    <target name="build">
        <delete dir="classes" failonerror="false"/>
        <mkdir dir="classes"/>
        <javac destdir="classes" includeAntRuntime="false" srcdir="src">
            <withKotlin/>
        </javac>
        <jar destfile="hello.jar">
            <fileset dir="classes"/>
        </jar>
    </target>
</project>
```
还可以将正在编译的模块的名称指定为 moduleName 属性:
```xml
<withKotlin moduleName="myModule"/>
```

# 面向 JavaScript 用单个源文件夹
```xml
<project name="Ant Task Test" default="build">
    <typedef resource="org/jetbrains/kotlin/ant/antlib.xml" classpath="${kotlin.lib}/kotlin-ant.jar"/>

    <target name="build">
        <kotlin2js src="root1" output="out.js"/>
    </target>
</project>
```

# 面向 JavaScript 用 Prefix、 PostFix 以及 sourcemap 选项
```xml
<project name="Ant Task Test" default="build">
    <taskdef resource="org/jetbrains/kotlin/ant/antlib.xml" classpath="${kotlin.lib}/kotlin-ant.jar"/>

    <target name="build">
        <kotlin2js src="root1" output="out.js" outputPrefix="prefix" outputPostfix="postfix" sourcemap="true"/>
    </target>
</project>
```

# 面向 JavaScript 用单个源文件夹以及 metaInfo 选项
如果要将翻译结果作为 Kotlin/JavaScript 库分发，那么 metaInfo 选项会很有用。 如果 metaInfo 设置为 true ，则在编译期间将创建具有二进制元数据的额外的 JS 文件。该文件
应该与翻译结果一起分发:

```xml
<project name="Ant Task Test" default="build">
    <typedef resource="org/jetbrains/kotlin/ant/antlib.xml" classpath="${kotlin.lib}/kotlin-ant.jar"/>
    
    <target name="build">
        <!-- 会创建 out.meta.js，其中包含二进制元数据 -->
        <kotlin2js src="root1" output="out.js" metaInfo="true"/>
    </target>
</project>
```

# 传递原始编译器参数
如需传递原始编译器参数，可以使用带 value 或 line 属性的 <compilerarg> 元素。可以放在 <kotlinc> 、 <kotlin2js> 与 <withKotlin> 任务元素内，如下所示:

```xml
<kotlinc src="${test.data}/hello.kt" output="${temp}/hello.jar"> 
    <compilerarg value="-Xno-inline"/>
    <compilerarg line="-Xno-call-assertions -Xno-param-assertions"/> 
    <compilerarg value="-Xno-optimize"/>
</kotlinc>
```

当运行 `kotlinc -help` 时，会显示可以使用的参数的完整列表。