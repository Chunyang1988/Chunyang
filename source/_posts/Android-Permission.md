---
title: Android 动态授权那些事
date: 2017-12-31 14:34:23
tags: [Android]
---


## 关键字
+ 动态授权
+ Android API

## 学习目标
了解动态授权机制，以及各个版本注意事项。文章结尾有项目地址。
## 正文
从 Android 6.0(API 23)开始，应用要使用危险全系时候，需要动态获取权限，而不是安装时候授权。

系统权限分为两类：**正常权限**和**危险权限**

+ 正常权限不会直接给用户隐私带来风险。如果在应用的AndroidManifes中注册，系统将会自动授予权限。
+ 危险权限会授予应用访问用户机密数据的权限，如果在应用的AndroidManifes中注册，则用户必须明确批准应用才能使用此权限。

运行时权限说明：


+ 如果设备运行在 Android5.1 或者更低版本，如果 AndroidManifest 列出危险权限，则用户必须安装应用时授予此权限，反之则无法安装应用。
+ 如果设备运行在 Android6.0 或者更高版本，应用必须在 AndroidManifest 中列出权限，并且它必须在运行时请求需要的危险权限。

### 流程
![](http://upload-images.jianshu.io/upload_images/2210217-ceda81027909ea98.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### 检测权限

如果使用到危险权限，则每次执行需要这一权限的操作时，必须都要检查是否已有权限。检测权限需要调用`PermissionChecker.checkSelfPermission(@NonNull Context context,@NonNull String permission) `方法
例如：

```
int permissionCheck = PermissionChecker.checkSelfPermission(thisActivity,
        Manifest.permission.WRITE_CALENDAR);
```
如果有此权限，方法返回`PackageManager.PERMISSION_GRANTED`，反之返回`PackageManager.PERMISSION_DENIED`

### 请求权限

如果应用无所需的权限，则应用必须调用`ActivityCompat.requestPermissions(final @NonNull Activity activity,final @NonNull String[] permissions, final @IntRange(from = 0) int requestCode)`方法来请求适当的权限。这时候会跳出系统对话框（不可定制），询问用户是否授权，系统会将结果会调给`Activity.onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults)`方法。

例如：

```
// Here, thisActivity is the current activity
if (ContextCompat.checkSelfPermission(thisActivity,
                Manifest.permission.READ_CONTACTS)
        != PackageManager.PERMISSION_GRANTED) {

    // Should we show an explanation?
    if (ActivityCompat.shouldShowRequestPermissionRationale(thisActivity,
            Manifest.permission.READ_CONTACTS)) {

        // Show an expanation to the user *asynchronously* -- don't block
        // this thread waiting for the user's response! After the user
        // sees the explanation, try again to request the permission.

    } else {

        // No explanation needed, we can request the permission.

        ActivityCompat.requestPermissions(thisActivity,
                new String[]{Manifest.permission.READ_CONTACTS},
                MY_PERMISSIONS_REQUEST_READ_CONTACTS);

        // MY_PERMISSIONS_REQUEST_READ_CONTACTS is an
        // app-defined int constant. The callback method gets the
        // result of the request.
    }
}
```

注：`ActivityCompat.shouldShowRequestPermissionRationale()`一般用它来判断，如果用户在过去拒绝了权限请求，并在权限请求系统对话框中选择了 Don't ask again 选项，此方法将返回 false。如果设备规范禁止应用具有该权限，此方法也会返回 false。因此用此方法用于解释应用为什么需要权限的情况，来提示用户权限使用。

### 处理权限请求

当应用请求权限后，系统会将结果会调给`onRequestPermissionsResult()`方法
例如：

```
@Override
public void onRequestPermissionsResult(int requestCode,
        String permissions[], int[] grantResults) {
    switch (requestCode) {
        case MY_PERMISSIONS_REQUEST_READ_CONTACTS: {
            // If request is cancelled, the result arrays are empty.
            if (grantResults.length > 0
                && grantResults[0] == PackageManager.PERMISSION_GRANTED) {

                // permission was granted, yay! Do the
                // contacts-related task you need to do.

            } else {

                // permission denied, boo! Disable the
                // functionality that depends on this permission.
            }
            return;
        }

        // other 'case' lines to check for other
        // permissions this app might request
    }
}
```
请求的权限结果会返回给此方法，其中`int[] grantResults`用户判断当时请求的权限`String permissions[]`结果，如果获取到权限则为`PackageManager.PERMISSION_GRANTED`其他都未未获取到权限。


详情请看[Android-Permission](https://github.com/Chunyang1988/Android-Permission)


