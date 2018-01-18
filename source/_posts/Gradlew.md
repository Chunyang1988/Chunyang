---
title: Gradle Wrapper 常用方法讲解
date: 2017-08-31 14:44:13
tags: [Android]
categories: [Android]
---

Gradle是一个基于Groovy强大的构建系统，此文讲解的是Android Studio所使用的一些常用命令。至于以后慢慢学习在讲解一些Gradle基础知识。


## 常用命令

以下命令都是在Mac系统下的常用命令。

```
./gradlew -v 		# 版本号
./gradlew clean		# 清理项目build文件夹
./gradlew build		# 检查依赖并编译打包,同时把debug、release环境的包都打出来。与上文build相对应，一个是清理，一个是生成。
./gradlew assembleDebug		# 编译并打Debug包
./gradlew assembleXxxRelease		# 编译并打Release的包
```

这里面在讲解一下`assemble`的其他使用方法。

```
./gradlew assembleXxxRelease	# 生成Xxx渠道的Release版本包
./gradlew assembleXxx	# 生成Xxx渠道的Release和Debug版本包
./gradlew assembleRelease	# 生成全部渠道的Release版本包。
```

Xxx为渠道名称写法如下：

```
android{
...
   productFlavors {
        tencent{
        ...
        ...
        }
        ...
   }
...
}
```

完整版本如下：

```
// 友盟多渠道打包
productFlavors {
    tencent {}
    wandoujia {}
    host {}
}

productFlavors.all { flavor ->
    flavor.manifestPlaceholders = [UMENG_CHANNEL_VALUE: name]
}  
```

```
// AndroidManifest.xml清单中如下

<meta-data
    android:name="UMENG_CHANNEL"
    android:value="${UMENG_CHANNEL_VALUE}" />
```




