---
title: Android Gradle 基础说明
date: 2017-12-24 15:34:01
tags: [Android , Gradle]
---

## Android Studio 目录结构

Android Studio 目录结构说明：

```
├── app #Android App目录
│   ├── app.iml
│   ├── build #构建输出目录
│   ├── build.gradle #构建脚本
│   ├── libs #so相关库
│   ├── proguard-rules.pro #proguard混淆配置
│   └── src #源代码，资源等
├── build
│   └── intermediates
├── build.gradle #工程构建文件
├── gradle
│   └── wrapper
├── gradle.properties #gradle的配置
├── gradlew #gradle wrapper linux shell脚本
├── gradlew.bat
├── LibSqlite.iml
├── local.properties #配置Androod SDK位置文件
└── settings.gradle #工程配置
```

**说明**

1. settings.gradle：
 用于配置 Project 中有几个 Module
 
2. build.gradle:
 + Projcet 目录设置为整个项目所使用的配置；
 + Module 目录当前项目所使用的配置；

## AndroidMainfest 占位符
在项目使用时候，可能或多或少会需要在 AndroidMainfest 里面配置一些特殊数据，例如友盟的渠道，第三方的 ApkKey 等信息，如： 

```
# 腾讯 APPID
<data android:scheme="tencent${TENCENT_KEY}" />

# 友盟渠道
<meta-data android:value="${UMENG_CHANNEL_VALUE}" android:name="UMENG_CHANNEL"/>
```

上面的 `${TENCENT_KEY}` 或 `${UMENG_CHANNEL_VALUE}` 就是占位符，我们替换的时候使用 `manifestPlaceholders = [UMENG_CHANNEL_VALUE: 'tencent']` 或`manifestPlaceholders.put("UMENG_CHANNEL_VALUE",'tencent')`


```
manifestPlaceholders = [UMENG_CHANNEL_VALUE: 'tencent']

manifestPlaceholders.put("UMENG_CHANNEL_VALUE",'tencent')
```

具体放在哪里使用manifestPlaceholders可以根据实际需求例如放入`buildTypes`、`defaultConfig`、`productFlavors`等等地方

## 自定义 BuildConfig
BuildConfig.java是 Android Gradle
自动编译生成的一个文件，我们也可以通过buildConfigField来自定义一些属性。


```
# String 类型
buildConfigField "String", "TENCENT_KEY", '"Value"'
# boolean 类型 
buildConfigField "boolean", "CONFIG", "true"
# int 类型    
buildConfigField "int", "VALUE", "1"  
```

这样BuildConfig.java就会有上面那自定义的属性

```
// Fields from default config.
public static final boolean CONFIG = true;
public static final String TENCENT_KEY = "Value";
public static final int VALUE = 1;
```

具体放在哪里使用，可以根据实际需求例如放入`buildTypes`、`defaultConfig`、`productFlavors`等等地方

## 统一依赖管理
统一依赖的目的是方便各个 Module 中引用版本升级时候防止多个 Module 改变而只需要更改这一个文件即可。

创建一个`config.gradle`文件

```
ext {

    android = [compileSdkVersion: 26,
               minSdkVersion    : 15,
               targetSdkVersion : 26
    ]

    dependencies = [
            "appcompat-v7": 'com.android.support:appcompat-v7:26.1.0'
    ]
}
```

在 Project 项目 build.gradle 中引用 

```
apply from: "config.gradle"
```

接下来只需要在使用的 build.gradle 中引用即可。

```
android {
    compileSdkVersion rootProject.ext.android.compileSdkVersion
    defaultConfig {

        minSdkVersion rootProject.ext.android.minSdkVersion
        targetSdkVersion rootProject.ext.android.targetSdkVersion

        applicationId "com.xiu8.apidemo"
        versionCode 1
        versionName "1.0"

        buildConfigField "String", "TENCENT_KEY", '"Value"'
        buildConfigField "boolean", "CONFIG", "true"
        buildConfigField "int", "VALUE", "1"
    }
    
    ...
    
}

dependencies {
    compile fileTree(include: ['*.jar'], dir: 'libs')
    compile rootProject.ext.dependencies["appcompat-v7"]
}
```

这样即可解决统一依赖管理，方便你随意升级版版本，而不用那个改其他 Module 









