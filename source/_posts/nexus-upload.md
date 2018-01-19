---
title: 上传库到私服（Nexus）
date: 2018-01-20 01:10:23
tags: [Tools]
---

这里面主要介绍 Android Studio 通过 gradle 上传库到私有 Maven 服务器 Nexus 的配置文件如何配置。
简单的介绍一些基础知识，详细的可以看文章结尾处的链接。

## 关键字
* Nexus
* Gradle
* Maven

## 学习目标
* 记录Nexus库的基础上传方式
* 理解基本定义

## 正文
Maven是什么？

Maven 是一个项目管理和构建自动化工具。

什么是构建工具？

构建工具是将软件项目构建相关的过程自动化的工具。构建一个软件项目通常包含以下一个或多个过程：

生成源码（如果项目使用自动生成源码）；
从源码生成项目文档；
编译源码；
将编译后的代码打包成JAR文件或者ZIP文件；
将打包好的代码安装到服务器、仓库或者其它的地方；

Nexus又是什么？

Nexus是一个基于maven的仓库管理的社区项目。

主要的使用场景就是可以在局域网搭建一个maven私服,用来部署第三方公共构件或者作为远程仓库在该局域网的一个代理。

先说明一下本文不会过多介绍名词解析，在这里主要是记录最基本的配置，防止博主忘记。

### 正文的正文

首先在项目的根目录的 `gradle.properties`中配置如下信息：

```
# 用户名
USERNAME=admin
# 密码
PASSWORD=admin123
# 仓库地址
NEXUS_REPOSITORY_URL=http://localhost:32770/repository/chunyang1988/
# 库的描述信息
DESCRIPTION=dependences lib for Android
# 格式aar、jar等
PACKAGING=aar
# 库的包名
GROUP_ID=com.cy
# 库的项目名称
ARTIFACT_ID=artifactId
# 库的名称
NAME=name
# 库的版本号
VERSION=1.0.0
```

一般在根目录创建如下`nexus_upload.gradle`上传task

```
apply plugin: 'maven'

task androidJavadocs(type: Javadoc) {
    source = android.sourceSets.main.java.srcDirs
    classpath += project.files(android.getBootClasspath().join(File.pathSeparator))
}

task androidJavadocsJar(type: Jar, dependsOn: androidJavadocs) {
    classifier = 'javadoc'
    from androidJavadocs.destinationDir
}

task androidSourcesJar(type: Jar) {
    classifier = 'sources'
    from android.sourceSets.main.java.srcDirs
}

artifacts {
    archives androidSourcesJar
    archives androidJavadocsJar
}

uploadArchives {
    repositories {
        mavenDeployer {
            repository(url: NEXUS_REPOSITORY_URL) {
                authentication(userName: USERNAME, password: PASSWORD)
            }
            pom.project {
                name NAME
                version VERSION
                artifactId ARTIFACT_ID
                groupId GROUP_ID
                packaging PACKAGING
                description DESCRIPTION
            }
        }
    }
}
```

也可以将`nexus_upload.gradle`上传到服务器中方便下载使用。

接下来就是使用了如下：

```
NAME = 'permision'
VERSION = '1.0.1'
ARTIFACT_ID = 'permision'
apply from: '../nexus_maven.gradle'//或者
apply from: 'http://xxx/nexus_maven.gradle'
```
之后就是运行此task即可，可以点击 AndroidStudio 右侧的 Gradle 选中相应树双击即可，也可以运行 `./gradlew uploadArchivesG
`即可。

到此基本基本记录已经完毕，更加深入的了解可以查看如下文章。



http://ifeve.com/maven-1/
https://www.sonatype.com/download-oss-sonatype

