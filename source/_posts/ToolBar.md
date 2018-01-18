---
title: Toolbar基本使用
date: 2017-08-18 22:26:05
tags: [Android]
---
此文讲解的Toolbar为V7包中的Toolbar。此文会有很多不成熟地方，请大家指正。

## 设置样式
首先使用Toolbar需要先更改Style样式，主要是将样式设置成无Actionbar的样式，当然网上也有很多其他方式，本人使用的代码如下：

```
	<style name="AppTheme" parent="AppTheme.Base">
        <!-- Customize your theme here. -->
        <item name="colorPrimary">@color/colorPrimary</item>
        <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
        <item name="colorAccent">@color/colorAccent</item>
    </style>


    <style name="AppTheme.Base" parent="Theme.AppCompat.Light.NoActionBar">

    </style>
```

## 官方文档中XML资源

![](http://upload-images.jianshu.io/upload_images/2210217-8f81e990effdea5b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![](http://upload-images.jianshu.io/upload_images/2210217-e3f8039a8db02bf8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 常用参数以及方法说明
### 1.基本使用


```
    <android.support.v7.widget.Toolbar
        android:id="@+id/toolbar"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="?attr/colorPrimary"
        tools:logo="@mipmap/ic_launcher"
        tools:navigationIcon="@drawable/ic_acb_list"
        tools:subtitle="@string/app_name"
        tools:subtitleTextColor="@color/colorAccent"
        tools:title="@string/app_name"
        tools:titleTextColor="@android:color/white">

    </android.support.v7.widget.Toolbar>
```

tools中的参数如下图解释
![](http://upload-images.jianshu.io/upload_images/2210217-8a2737cb975ed701.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

代码实现如下：

```
        Toolbar toolbar = (Toolbar) findViewById(R.id.toolbar);

        toolbar.setNavigationIcon(R.drawable.ic_acb_list);//设置导航栏图标
        toolbar.setLogo(R.mipmap.ic_launcher);//设置app logo
        toolbar.setTitle("Toolbar");//设置主标题
        toolbar.setTitleTextColor(ContextCompat.getColor(this, android.R.color.white));
        toolbar.setSubtitle("Subtitle");//设置子标题
        toolbar.setSubtitleTextColor(ContextCompat.getColor(this, R.color.colorAccent));
```

### 2.菜单使用

主代码部分

```
        toolbar.setOverflowIcon(ContextCompat.getDrawable(this, R.drawable.ic_acb_list));//设置更多图标

        toolbar.inflateMenu(R.menu.main_menu);
        toolbar.setOnMenuItemClickListener(new Toolbar.OnMenuItemClickListener() {
            @Override
            public boolean onMenuItemClick(MenuItem item) {
                int id = item.getItemId();
                Toast.makeText(MainActivity.this, String.valueOf(id), Toast.LENGTH_SHORT).show();
                switch (id) {
                    case R.id.action_1:
                        break;
                    case R.id.action_2:
                        break;
                    case R.id.action_3:
                        break;
                    case R.id.action_4:
                        break;
                    case R.id.action_5:
                        break;
                }
                return false;
            }
        });
```

menu部分

```
	<menu xmlns:android="http://schemas.android.com/apk/res/android"
    	xmlns:app="http://schemas.android.com/apk/res-auto">

	    <item
	        android:id="@+id/action_1"
	        android:icon="@mipmap/ic_launcher"
	        android:title="@string/app_name"
	        app:showAsAction="never"></item>

	    <item
	        android:id="@+id/action_2"
	        android:icon="@mipmap/ic_launcher"
	        android:title="@string/app_name"
	        app:showAsAction="never"></item>
	    <item
	        android:id="@+id/action_3"
	        android:icon="@mipmap/ic_launcher"
	        android:title="@string/app_name"
	        app:showAsAction="never"></item>
	    <item
	        android:id="@+id/action_4"
	        android:icon="@mipmap/ic_launcher"
	        android:title="@string/app_name"
	        app:showAsAction="never"></item>
	    <item
	        android:id="@+id/action_5"
	        android:icon="@mipmap/ic_launcher"
	        android:title="@string/app_name"
	        app:showAsAction="never"></item>
	</menu>
```


![](http://upload-images.jianshu.io/upload_images/2210217-51f9e6ee5319c0e0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![](http://upload-images.jianshu.io/upload_images/2210217-e2a29d76964f995b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 3.特殊说明

A.TextAppearnce

Toolbar当然也可以自定义样式，例如自定义Title字体大小,字体颜色等等基本信息，这时候会使用到一个属性`TextAppearance`

TextAppearance：可以从新定义Style，接着看代码：

```
    <style name="Toolbar.Title" parent="@style/TextAppearance.Widget.AppCompat.Toolbar.Title">
        <item name="android:textSize">22sp</item>
        <item name="android:textColor">@android:color/white</item>
    </style>
```

其中上面中将字体大小已经字体颜色都改变。
特别说明，如果你前面已经定义了颜色后面又调用了TextAppearance，那面你之前设置都按照你新定义的Appearance来实现.

B.PopupStyle

```
    <style name="Toolbar.Popup" parent="@style/Widget.AppCompat.PopupMenu.Overflow">
        <item name="overlapAnchor">false</item>
    </style>
```

默认情况overlapAnchor为true，而这样在弹出更多菜单的时候，会覆盖标题栏弹出，如果这是false则不会覆盖标题栏。

