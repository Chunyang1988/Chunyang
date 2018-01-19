---
title: Android各种SdkVersion理解
date: 2017-11-9 16:26:33
tags: [Android]
---


## 关键字
* minSdkVersion
* compileSdkVersion
* targetSdkVersion

## 学习目标
了解各种sdkVersion具体意思。
## 正文
Google 官方发布[文章](https://medium.com/google-developers/picking-your-compilesdkversion-minsdkversion-targetsdkversion-a098a0341ebd#.egywqatjg) 解析compileSdkVersion、minSdkVersion 以及 targetSdkVersion 的含义，以及合理设置各个值的意义，还有[翻译](https://chinagdg.org/2016/01/picking-your-compilesdkversion-minsdkversion-targetsdkversion/)
### minSdkVersion
设置应用可运行最低版本，如果系统的 API 高于该值，则系统会阻止程序安装。
### compileSdkVersion
设置编辑版本，也就是告诉 Gradle 用哪个 Android SDK 版本编译你的应用，一般都是选择最新的SDK。

当你修改了 compileSdkVersion 的时候，可能会出现新的编译警告、编译错误，但新的 compileSdkVersion 不会被包含到 APK 中：它纯粹只是在编译的时候使用。（你真的应该修复这些警告，他们的出现一定是有原因的）。

在这里需要注意的是，编译报错的时候，一定要得到注意，根据提示来做兼容设置，例如判断Build.VERSION_SDK_INT常量表示当前Android设备的版本号来比对，使用不同的兼容方案。
### targetSdkVersion
targetSdkVersion 是 Android 提供向前兼容的主要依据，**在应用的 targetSdkVersion 没有更新之前系统不会应用最新的行为变化**。

怎么理解这句话呢？如果设备API等于 targetSdkVersion 就是告知当前项目已经兼容新的行为变化。
