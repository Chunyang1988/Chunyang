---
title: App启动优化
date: 2017-8-11 22:49:33
tags: [Android]
---

## 启动白屏
每次令启动的时候，会出现一个白屏现象
引起原因：
1.Application的onCreate做了大量初始化操作；
建议：可以放到开始使用的地方初始化操作；
2.Activity的onCreate中有很多复杂布局与渲染操作；
建议：仅初始化自己需要的对象，xml布局减少嵌套布局；

## 优化方案

1.关闭启动窗口

建立style

```
    <style name="AppTheme.Launcher" parent="AppTheme">
        <item name="android:windowDisablePreview">true</item>
    </style>
```
引用style，只需要在MAIN中引用

```
 <activity
    android:theme="@style/AppTheme.Launcher"
    />
```

这样做虽然没有白屏了，但是会出现点击桌面图标不会立即反应的现象。

2.使用Material Design规范

建立style

```
    <style name="AppTheme.Launcher.MD">
        <item name="android:windowBackground">@drawable/launch_material_design</item>
    </style>
```

这里面更上面不同的就是使用layer-list方式制作一个简单启动页面


