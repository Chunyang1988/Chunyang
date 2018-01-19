---
title: Fragment基础知识
date: 2017-11-19 17:54:34
tags: [Android]
---

Fragment不能独立存在，它必须嵌入到activity中，而且Fragment的生命周期直接受所在的activity的影响。例如：当activity暂停时，它拥有的所有的Fragment们都暂停了，当activity销毁时，它拥有的所有Fragment们都被销毁。





![](https://upload-images.jianshu.io/upload_images/2210217-8d748ff4a0e8a124.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/500)





此文章没有任何讲解，只是纯粹的代码文档，列举几种方法，忘记的时候方便查看巩固使用的。

### 方法一：

在activity的layoutxml文件中声明fragment

```
<LinearLayoutxmlns:android="http://schemas.android.com/apk/res/android"
 android:layout_width="match_parent"
 android:layout_height="match_parent"
 android:baselineAligned="false">

   <fragment
     android:id="@+id/titles"
     android:layout_width="match_parent"
     android:layout_height="match_parent"
     android:layout_weight="4"
     class="cn.eoe.first.fragment.LeftFragment"/>

   <fragment
     android:id="@+id/details"
     android:layout_width="match_parent"
     android:layout_height="match_parent"
     android:layout_weight="1"
     class="cn.eoe.first.fragment.RightFragment"/>

</LinearLayout>

class= "cn.eoe.first.fragment.LeftFragment" 换成
android:name="cn.eoe.first.fragment.LeftFragment"也可以 

```

### 方法二：在代码中添加fragment到一个ViewGroup

```
   //先获得Fragment的管理
    FragmentManager fragmentManager = getFragmentManager();
   //所有Fragment的事务都是通过FragmentTransaction来完成，在通过管理者获取事务对象
    FragmentTransaction fragmentTransaction = fragmentManager
                     .beginTransaction();
   //实例化要添加的Fragment
    MyFragment fragment =  new  MyFragment();
   //添加Fragment通过layout中的id，实例对象，还有tag标签
     fragmentTransaction.add(R.id.fragment_container1, fragment,"fragment");
    //提交
    fragmentTransaction.commit();

```

或者

```
    FragmentTransaction tx =getSupportFragmentManager().beginTransaction();
    tx.replace(R.id.main, Fragment.instantiate(MyHomeSlidingActivity.**this**,
               "com.joymis.audio.FragmentmyhomeInfo"));
    tx.commit();

```

其中 "com.joymis.audio.FragmentmyhomeInfo"的代码

```
    public   class  FragmentmyhomeInfo  extends  Fragment {

   @Override
     public  View onCreateView(LayoutInflater inflater, ViewGroup container,
              Bundle savedInstanceState) {
         View view = inflater.inflate(R.layout.layout_myhome_fragment, container,**false**);

           return  view;
   }

   @Override
     public    void  onDestroyView() {

           super.onDestroyView();
     }

}

```

layout中的布局为

```
  <FrameLayout
     android:id="@+id/fragment_container1"
     android:layout_width="match_parent"
     android:layout_height="wrap_content" />

```

### 其他

要管理fragment们，需使用FragmentManager，要获取它，需在activity中调用方法getFragmentManager()。
你可以用FragmentManager来做以上事情：
1使用方法findFragmentById()或findFragmentByTag()，获取activity中已存在的fragment们。
2使用方法popBackStack()从activity的后退栈中弹出fragment们（这可以模拟后退键引发的动作）。
3用方法addOnBackStackChangedListerner()注册一个侦听器以监视后退栈的变化。






