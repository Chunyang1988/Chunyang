---
title: Android动态授权那些事
date: 2017-08-25 22:02:33
tags: [Android]
---

Android6.0(API 23)以后推出了很多新特性，其中 动态授权 对于开发人员来说带了很多麻烦，此文将说说动态授权那些事。

好在google已经给出了解决方案，答案都在support-v4-24.1.0包中。首先最重要的两个类ActivityCompat，ContextCompat。

## ContextCompat类
检测是否有相应权限permission，如果有则返回PackageManager.PERMISSION_DENIED。

    public static int checkSelfPermission(@NonNull Context context, @NonNull String permission)

## ActivityCompat类

请求权限

    public static void requestPermissions(final @NonNull Activity activity,final @NonNull String[] permissions, final int requestCode)

## Activity类返回

请求权限返回

     public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults)

## 说明
关于动态授权，核心代码无非就上面几个方法的使用。要想明白如何来动态获取权限，首先要知道其交互流程。

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/2210217-ceda81027909ea98.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


按照上图所示，分解步骤
1.检查所需权限是否获取
2.请求所有权限
3.友好提示用户，手动获取权限方法。
4.结束

## 步骤方法
### a.检查权限是否获取使用

    // 判断权限集合
    public boolean checkPermissions(String... permissions) {
        for (String permission : permissions) {
            if (checkPermission(context, permission))
                return true;
        }
        return false;
    }

    // 判断是否缺少权限
    private boolean checkPermission(Context context, String permission) {
        return ContextCompat.checkSelfPermission(context, permission) ==
                PackageManager.PERMISSION_DENIED;
    }
    
如果有为获取权限则请求权限

### b.请求权限

    // 请求权限兼容低版本
    public void requestPermissions(String... permissions) {
        ActivityCompat.requestPermissions(activity, permissions, PERMISSION_REQUEST_CODE);
    }

使用系统权限对话框提示用户是否授权，如有拒绝获取权限的地方，则优化提示用户手动获取权限方法。

### c.手动获取权限提示。

    private void showPermissionDialog() {
        AlertDialog.Builder builder = new AlertDialog.Builder(activity);
        builder.setTitle("帮助");
        builder.setMessage("当前应用缺少必要权限。\n\n请点击\"设置\"-\"权限\"-打开所需权限。");///n/n最后点击两次后退按钮，即可返回。

        // 拒绝, 退出应用
        builder.setNegativeButton("退出", new DialogInterface.OnClickListener() {
            @Override
            public void onClick(DialogInterface dialog, int which) {
                activity.setResult(PERMISSIONS_DENIED);
                activity.finish();
            }
        });

        builder.setPositiveButton("设置", new DialogInterface.OnClickListener() {
            @Override
            public void onClick(DialogInterface dialog, int which) {
                startAppSettings();
            }
        });

        builder.setCancelable(false);

        builder.show();
    }

    // 启动应用的设置
    private void startAppSettings() {
        Intent intent = new Intent(Settings.ACTION_APPLICATION_DETAILS_SETTINGS);
        intent.setData(Uri.parse(PACKAGE_URL_SCHEME + activity.getPackageName()));
        activity.startActivity(intent);
    }

### d.结束


## 使用

### a.初始化

	mCheckPermission = new CheckPermission(this) {
		@Override
		String[] getPermissions() {
			return new String[]{
					Manifest.permission.CAMERA
			};
		}
	};

b.检查权限

    @Override
    protected void onResume() {
        super.onResume();
        mCheckPermission.checkPermission();
    }

c.实现onRequestPermissionsResult

    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults) {
        super.onRequestPermissionsResult(requestCode, permissions, grantResults);
        mCheckPermission.checkRequestPermissionsResult(requestCode, grantResults);
    }

具体代码可以看我的[ZXingScan](https://github.com/Chunyang1988/ZXingScan)

PS:欢迎大家支持我的，[Github](https://github.com/Chunyang1988)

