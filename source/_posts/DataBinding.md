---
title: DataBinding
date: 2017-08-27 22:36:30
tags: [Android]
---
此文讲解的是MVVM中的Data Binding数据绑定

## 打开数据绑定
在Model的gradle中添加：

```
android {
    ....
    dataBinding {
        enabled = true
    }
}
```

## Data Binding Layout说明

使用Data Binding后，可以将UI代码放到xml中，布局和数据更加紧密。

xml文件中，根节点为layout，之后data标签，在下面就是常用界面布局文件。


```
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android">
   <data>
       <variable name="user" type="com.example.User"/>
   </data>
   <LinearLayout
       android:orientation="vertical"
       android:layout_width="match_parent"
       android:layout_height="match_parent">
       <TextView android:layout_width="wrap_content"
           android:layout_height="wrap_content"
           android:text="@{user.firstName}"/>
       <TextView android:layout_width="wrap_content"
           android:layout_height="wrap_content"
           android:text="@{user.lastName}"/>
   </LinearLayout>
</layout>
```

### data标签常用属性
**class属性**
`class` 自定义Binding名称，如果不写则默认是驼峰xml文件名+Binding，例如activity_main.xml则为ActivityMainBinding。如果有`class = "MainViewBinding"` 则直接这这个名字。

**variable标签**
用于描述一个对象，之后在布局中引用使用，使用`name`表示对象引用时使用名称，`type`为要引用的对象。在View中用`@{}`来引用。例如

```
<variable name = "user" type = "com.example.User"/>

...

<TextView 
    android:layout_width = "warp_content"
    android:layout_hight = "warp_content"
    android:text = "@{user.name}"/>
```

**import标签**
导入跟Java用法一样，只要导入了当前类，后面引用的时候，就可以直接使用，不需要在制定路径例如：

```
<data>
    <import type="android.view.View"/>
</data>
...
<TextView
   android:text="@{user.lastName}"
   android:layout_width="wrap_content"
   android:layout_height="wrap_content"
   android:visibility="@{user.isAdult ? View.VISIBLE : View.GONE}"/>
```

```
<data>
    <import type="com.example.MyStringUtils"/>
    <variable name="user" type="com.example.User"/>
</data>
…
<TextView
   android:text="@{MyStringUtils.capitalize(user.lastName)}"
   android:layout_width="wrap_content"
   android:layout_height="wrap_content"/>
   
```

## Binding 数据对象
创建正确的数据对象才能在xml文件中引用，反之则会报错。
主要就是对属性进行getXX、isXX等。

## 运算符

* 算术 + - / * %
* 字符串合并 +
* 逻辑 && ||
* 二元 & | ^
* 一元 + - ! ~
* 移位 >> >>> <<
* 比较 == > < >= <=
* Instanceof
* Grouping ()
* 文字 - character, String, numeric, null
* Cast
* 方法调用
* Field 访问
* Array 访问 []
* 三元 ?:

尚且不支持this,super,new,以及显示的泛型，还支持一种空合并运算符

```
android:text = "@{user.displayName ?? user.lastName}"
```
还可以使用

```
android:text="@{@string/nameFormat(firstName, lastName)}"

<string name="nameFormat">%s, %s</string>
```
## 事件监听
事件监听两种实现方式

1. 方法引用
    主要是使用`名称::方法名`格式，而Binding类中方法必须有View参数，要不然编译不通过。
    
    ```
    public class Handlers {
        public void onClickFriend(View view) { ... }
    }
    
    ····
    <?xml version="1.0" encoding="utf-8"?>
    <layout xmlns:android="http://schemas.android.com/apk/res/android">
       <data>
           <variable name="handlers" type="com.example.Handlers"/>
           <variable name="user" type="com.example.User"/>
       </data>
       <LinearLayout
           android:orientation="vertical"
           android:layout_width="match_parent"
           android:layout_height="match_parent">
           <TextView android:layout_width="wrap_content"
               android:layout_height="wrap_content"
               android:text="@{user.firstName}"
               android:onClick="@{handlers::onClickFriend}"/>
       </LinearLayout>
    </layout>
    ```

2. 监听器绑定
使用`() -> 名称.方法(参数...)`

```
public class Presenter {
    public void onSaveClick(Task task){}
}
···
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android">
    <data>
        <variable name="task" type="com.android.example.Task" />
        <variable name="presenter" type="com.android.example.Presenter" />
    </data>
    <LinearLayout android:layout_width="match_parent" android:layout_height="match_parent">
        <Button android:layout_width="wrap_content" android:layout_height="wrap_content"
        android:onClick="@{() -> presenter.onSaveClick(task)}" />
    </LinearLayout>
</layout>
```

## ID Views
只要是在xml中，带有id的View都会在Binding中通过public final 来修饰。直接调用也可以达到findViewById效果。

## Observable 
动态更新数据
    
http://www.jianshu.com/p/b1df61a4df77
http://blog.zhaiyifan.cn/2016/06/16/android-new-project-from-0-p7/
http://www.jianshu.com/p/87d4b9f30960








