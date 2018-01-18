---
title: ADB 常用命令
date: 2017-10-18 17:43:34
tags: [Android]
---


重启机器

```
adb reboot
```
重启机器到bootloader刷机模式

```
adb reboot bootloader
```
重启机器到recovery恢复模式

```
adb reboot recovery
```
查看设备应用

```
adb shell pm ls packages //查看设备所有应用
adb shell pm list packages -s //查看系统应用
adb shell pm list packages -3 //查看第三方应用
adb shell pm list packages <name> //查看包含<name>列表
或者
adb shell pm list packages | grep <name>
```
查看应用详情

```
adb shell dumpsys package <packagename>
```
查看设备分辨率

```
adb shell wm size
adb shell wm size 480x1024 //修改分辨率
adb shell wm size reset //恢复分辨率
```
查看屏幕密度

```
adb shell wm density
adb shell wm density 160 //修改屏幕密度
adb shell wm density reset //回复屏幕密度
```
查看系统版本

```
adb shell getprop ro.build.version.release
```
强制停止应用

```
adb shell am force-stop <packagename>
```

复制设备文件到电脑
```
adb pull <设备里的文件路径> [电脑上的目录]
```

复制电脑文件到设备

```
adb push <电脑上的文件路径> <设备里的目录>
```
查看log日志

```
adb logcat
```
查看内核日志

```
adb shell dmesg
```


安装APk

```
adb install [-lrtsdg] <path_to_apk>// adb install x.apk
```
| 参数 | 含义                                                                              |
|------|-----------------------------------------------------------------------------------|
| -l   | 将应用安装到保护目录 /mnt/asec                                                    |
| -r   | 允许覆盖安装                                                                      |
| -t   | 允许安装 AndroidManifest.xml 里 application 指定 `android:testOnly="true"` 的应用 |
| -s   | 将应用安装到 sdcard                                                               |
| -d   | 允许降级覆盖安装                                                                  |
| -g   | 授予所有运行时权限                                                                |


常见安装失败输出代码、含义及可能的解决办法如下：

| 输出                                                                | 含义                                                                     | 解决办法                                                                       |
|---------------------------------------------------------------------|--------------------------------------------------------------------------|--------------------------------------------------------------------------------|
| INSTALL\_FAILED\_ALREADY\_EXISTS                                    | 应用已经存在，或卸载了但没卸载干净                                       | `adb install` 时使用 `-r` 参数，或者先 `adb uninstall <packagename>` 再安装    |
| INSTALL\_FAILED\_INVALID\_APK                                       | 无效的 APK 文件                                                          |                                                                                |
| INSTALL\_FAILED\_INVALID\_URI                                       | 无效的 APK 文件名                                                        | 确保 APK 文件名里无中文                                                        |
| INSTALL\_FAILED\_INSUFFICIENT\_STORAGE                              | 空间不足                                                                 | 清理空间                                                                       |
| INSTALL\_FAILED\_DUPLICATE\_PACKAGE                                 | 已经存在同名程序                                                         |                                                                                |
| INSTALL\_FAILED\_NO\_SHARED\_USER                                   | 请求的共享用户不存在                                                     |                                                                                |
| INSTALL\_FAILED\_UPDATE\_INCOMPATIBLE                               | 以前安装过同名应用，但卸载时数据没有移除；或者已安装该应用，但签名不一致 | 先 `adb uninstall <packagename>` 再安装                                        |
| INSTALL\_FAILED\_SHARED\_USER\_INCOMPATIBLE                         | 请求的共享用户存在但签名不一致                                           |                                                                                |
| INSTALL\_FAILED\_MISSING\_SHARED\_LIBRARY                           | 安装包使用了设备上不可用的共享库                                         |                                                                                |
| INSTALL\_FAILED\_REPLACE\_COULDNT\_DELETE                           | 替换时无法删除                                                           |                                                                                |
| INSTALL\_FAILED\_DEXOPT                                             | dex 优化验证失败或空间不足                                               |                                                                                |
| INSTALL\_FAILED\_OLDER\_SDK                                         | 设备系统版本低于应用要求                                                 |                                                                                |
| INSTALL\_FAILED\_CONFLICTING\_PROVIDER                              | 设备里已经存在与应用里同名的 content provider                            |                                                                                |
| INSTALL\_FAILED\_NEWER\_SDK                                         | 设备系统版本高于应用要求                                                 |                                                                                |
| INSTALL\_FAILED\_TEST\_ONLY                                         | 应用是 test-only 的，但安装时没有指定 `-t` 参数                          |                                                                                |
| INSTALL\_FAILED\_CPU\_ABI\_INCOMPATIBLE                             | 包含不兼容设备 CPU 应用程序二进制接口的 native code                      |                                                                                |
| INSTALL\_FAILED\_MISSING\_FEATURE                                   | 应用使用了设备不可用的功能                                               |                                                                                |
| INSTALL\_FAILED\_CONTAINER\_ERROR                                   | 1. sdcard 访问失败;<br />2. 应用签名与 ROM 签名一致，被当作内置应用。    | 1. 确认 sdcard 可用，或者安装到内置存储;<br />2. 打包时不与 ROM 使用相同签名。 |
| INSTALL\_FAILED\_INVALID\_INSTALL\_LOCATION                         | 1. 不能安装到指定位置;<br />2. 应用签名与 ROM 签名一致，被当作内置应用。 | 1. 切换安装位置，添加或删除 `-s` 参数;<br />2. 打包时不与 ROM 使用相同签名。   |
| INSTALL\_FAILED\_MEDIA\_UNAVAILABLE                                 | 安装位置不可用                                                           | 一般为 sdcard，确认 sdcard 可用或安装到内置存储                                |
| INSTALL\_FAILED\_VERIFICATION\_TIMEOUT                              | 验证安装包超时                                                           |                                                                                |
| INSTALL\_FAILED\_VERIFICATION\_FAILURE                              | 验证安装包失败                                                           |                                                                                |
| INSTALL\_FAILED\_PACKAGE\_CHANGED                                   | 应用与调用程序期望的不一致                                               |                                                                                |
| INSTALL\_FAILED\_UID\_CHANGED                                       | 以前安装过该应用，与本次分配的 UID 不一致                                | 清除以前安装过的残留文件                                                       |
| INSTALL\_FAILED\_VERSION\_DOWNGRADE                                 | 已经安装了该应用更高版本                                                 | 使用 `-d` 参数                                                                 |
| INSTALL\_FAILED\_PERMISSION\_MODEL\_DOWNGRADE                       | 已安装 target SDK 支持运行时权限的同名应用，要安装的版本不支持运行时权限 |                                                                                |
| INSTALL\_PARSE\_FAILED\_NOT\_APK                                    | 指定路径不是文件，或不是以 `.apk` 结尾                                   |                                                                                |
| INSTALL\_PARSE\_FAILED\_BAD\_MANIFEST                               | 无法解析的 AndroidManifest.xml 文件                                      |                                                                                |
| INSTALL\_PARSE\_FAILED\_UNEXPECTED\_EXCEPTION                       | 解析器遇到异常                                                           |                                                                                |
| INSTALL\_PARSE\_FAILED\_NO\_CERTIFICATES                            | 安装包没有签名                                                           |                                                                                |
| INSTALL\_PARSE\_FAILED\_INCONSISTENT\_CERTIFICATES                  | 已安装该应用，且签名与 APK 文件不一致                                    | 先卸载设备上的该应用，再安装                                                   |
| INSTALL\_PARSE\_FAILED\_CERTIFICATE\_ENCODING                       | 解析 APK 文件时遇到 `CertificateEncodingException`                       |                                                                                |
| INSTALL\_PARSE\_FAILED\_BAD\_PACKAGE\_NAME                          | manifest 文件里没有或者使用了无效的包名                                  |                                                                                |
| INSTALL\_PARSE\_FAILED\_BAD\_SHARED\_USER\_ID                       | manifest 文件里指定了无效的共享用户 ID                                   |                                                                                |
| INSTALL\_PARSE\_FAILED\_MANIFEST\_MALFORMED                         | 解析 manifest 文件时遇到结构性错误                                       |                                                                                |
| INSTALL\_PARSE\_FAILED\_MANIFEST\_EMPTY                             | 在 manifest 文件里找不到找可操作标签（instrumentation 或 application）   |                                                                                |
| INSTALL\_FAILED\_INTERNAL\_ERROR                                    | 因系统问题安装失败                                                       |                                                                                |
| INSTALL\_FAILED\_USER\_RESTRICTED                                   | 用户被限制安装应用                                                       |                                                                                |
| INSTALL\_FAILED\_DUPLICATE\_PERMISSION                              | 应用尝试定义一个已经存在的权限名称                                       |                                                                                |
| INSTALL\_FAILED\_NO\_MATCHING\_ABIS                                 | 应用包含设备的应用程序二进制接口不支持的 native code                     |                                                                                |
| INSTALL\_CANCELED\_BY\_USER                                         | 应用安装需要在设备上确认，但未操作设备或点了取消                         | 在设备上同意安装                                                               |
| INSTALL\_FAILED\_ACWF\_INCOMPATIBLE                                 | 应用程序与设备不兼容                                                     |                                                                                |
| does not contain AndroidManifest.xml                                | 无效的 APK 文件                                                          |                                                                                |
| is not a valid zip file                                             | 无效的 APK 文件                                                          |                                                                                |
| Offline                                                             | 设备未连接成功                                                           | 先将设备与 adb 连接成功                                                        |
| unauthorized                                                        | 设备未授权允许调试                                                       |                                                                                |
| error: device not found                                             | 没有连接成功的设备                                                       | 先将设备与 adb 连接成功                                                        |
| protocol failure                                                    | 设备已断开连接                                                           | 先将设备与 adb 连接成功                                                        |
| Unknown option: -s                                                  | Android 2.2 以下不支持安装到 sdcard                                      | 不使用 `-s` 参数                                                               |
| No space left on device                                             | 空间不足                                                                 | 清理空间                                                                       |
| Permission denied ... sdcard ...                                    | sdcard 不可用                                                            |                                                                                |
| signatures do not match the previously installed version; ignoring! | 已安装该应用且签名不一致                                                 | 先卸载设备上的该应用，再安装                                                   |


卸载apk

```
adb uninstall [-k] <packagename>
```
清除应用数据与缓存

```
adb shell pm clear <packagename>
//<packagename> 表示应用名包，这条命令的效果相当于在设置里的应用信息界面点击了「清除缓存」和「清除数据
```








还有部分功能是需要root的以及更多命令可以查看[github](https://github.com/mzlogin/awesome-adb)




