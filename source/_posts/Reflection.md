---
title: Java反射机制
date: 2018-01-25 15:41:31
tags: [Android]
---

## 关键字

* 反射
* 动态化

## 学习目标

* 了解 Java 反射主要类
* 了解 Java 反射常用功能
* 了解 Java 反射功能与场景

## 正文
Java 反射是Java语言的一个很重要的特征，它使得Java具体了“动态性”。

Java 反射机制主要提供了以下功能：

* 在运行时判断任意一个对象所属的类。
* 在运行时构造任意一个类的对象。
* 在运行时判断任意一个类所具有的成员变量和方法。
* 在运行时调用任意一个对象的方法。


### Java 反射主要类
主要由一下类来实现 Java 反射机制：

* Class类：代表一个类。
* Field 类：代表类的成员变量（成员变量也称为类的属性）。
* Method类：代表类的方法。
* Constructor 类：代表类的构造方法。
* Array类：提供了动态创建数组，以及访问数组的元素的静态方法。

### 特殊方法讲解
#### getField 与 getDeclaredField
Field代表类的成员变量

**getField**
获取类中`public`的成员变量，包含基类（父类）的成员变量。
**getDeclaredField**
获取类中所有成员变量，但不包含基类（父类）的成员变量。
#### getMethods 与 getDeclaredMethods
Method代表类的方法
**getMethods**
获取类中`public`的方法，包含基类（父类）的方法。
**getDeclaredMethods**
获取类中所有方法，但不包含基类（父类）的方法。

#### setAccessible
在获取为`private`修饰的的时候，需要使用此方法，才能获取到。
#### PS：
上文中用到的 `private`、`public` 都是必要条件，例如 `setAccessible` 中的 `private`中，只有 `private` 的方法或者属性才用添加，非 `private` 则无需添加。

## 实例讲解
由于内存存储情况，在获取类属性或者方法的时候，都是需要分情况考虑因此都会有**非静态类**与**静态类**区分。
下面的实例会用到几个类具体情况在文章最后会给出
### 获取类属性
在非静态类中，想要获取某个属性，需要知道在哪个对象中，而静态类中则只需要知道那个 Class 即可。
#### 非静态类

例如下文中的 sample 对象，通过反射就能知道 private 修饰的 tag 值是多少。

```
Sample sample = new Sample();
Class ownerClass = sample.getClass();
Field field = ownerClass.getDeclaredField("TAG");
field.setAccessible(true);
Object property = field.get(ownerClass);
```


上面例子可以提取出来这样的一个方法

```
public static <T> T getDeclaredField(Object owner, String fieldName, boolean isAccessible) throws Exception {
    Class ownerClass = owner.getClass();
    Field field = ownerClass.getDeclaredField(fieldName);
    field.setAccessible(isAccessible);
    return (T) field.get(owner);
}
```
#### 静态类

在静态类中，由于内存情况，因此无需知道对象，只需要知道那个 class 即可获取到想要的属性。

```
Class ownerClass = Sample.class;
Field field = ownerClass.getDeclaredField("TAG");
field.setAccessible(true);
Object property = field.get(ownerClass);
```
上面例子可以提取出来这样的一个方法

```
public static <T> T getDeclaredField(Class cls, String fieldName, boolean isAccessible) throws Exception {
    Field field = cls.getDeclaredField(fieldName);
    field.setAccessible(isAccessible);
    return (T) field.get(cls);
}
```

### 执行类属性
在非静态类中，想要执行某个方法，需要知道在哪个对象中，而静态类中则只需要知道那个 Class 即可。
#### 非静态类

```
Sample sample = new Sample();
Class ownerClass = sample.getClass();
Object arg = "newName";
Class argCls = arg.getClass();
Method method = ownerClass.getDeclaredMethod("getSample", argCls);
method.setAccessible(true);
Object object = method.invoke(sample, arg);
System.out.println("类属性：" + object);
```

#### 静态类

```
Class ownerClass = Sample.class;
Method method = ownerClass.getDeclaredMethod("getName");
method.setAccessible(true);
Object property = method.invoke(ownerClass);
System.out.println(property);
```

### 创建实例
实例化对象需要区分**带参数**与**无参数**
#### 带参数

```
Class newClass = Sample.class;
Object arg = "SampleName";
Class argClass = arg.getClass();
Constructor cons = newClass.getConstructor(argClass);
Sample sample = (Sample) cons.newInstance(arg);
System.out.println(sample.name);
```

#### 无参数

```
Class newClass = Sample.class;
Sample sample = (Sample) newClass.newInstance();
System.out.println(sample.name);
```


### 文中实例代码
Demo类代码

```
public class Demo {
    private int code = 1;
    public String versionName = "VersionName";
    protected int sdkVersion = 21;
    int minSdkVersion = 14;

    private String getVersionName() {
        return versionName;
    }

    public int getSdkVersion() {
        return sdkVersion;
    }

    int getMinSdkVersion() {
        return minSdkVersion;
    }
}
```
Sample类代码

```
public class Sample extends Demo {


    public Sample() {

    }

    public Sample(String name) {
        this.name = name;
    }

    private static final String TAG = "SampleTag";

    public String name = "Sample.Demo";
    private int age = 10;

    boolean isSample = true;

    private String getSample(String name) {
        System.out.println("getSample有参数");
        return "SampleName:" + name;
    }

    private String getSample() {
        System.out.println("getSample无参数");
        return "Sample";
    }

    private static String getName() {
        System.out.println("getName静态方法");
        return "Name";
    }

}
```



















