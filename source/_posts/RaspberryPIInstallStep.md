---
title: 树莓派3在Mac中安装步骤
date: 2017-9-02 19:13:01
tags: [Raspberry PI , 树莓派] 
---

1.格式TF卡
磁盘工具->抹掉TAB->MS_DOS(FAT)

2.查看挂载信息
命令行中输入 `df -h` 查看相对应的TF卡路径如：`/dev/disk2s1`

3.卸载分区
命令行输入`diskutil numount` 

4.确定设备
命令行输入`diskutil list` 主要目的是查看对应的路径名如：`/dev/disk2`

5.写入系统
```
sudo dd bs=4m if=固件名称.img of=上面确定的路径名
```

例如：
`sudo dd bs=4m if=LCD35-2016-03-18-raspbian-jessie.img of=/dev/rdisk2`

6.卸载设备
使用磁盘工具进行卸载即可