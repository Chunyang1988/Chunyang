---
title: Docker 安装 Nexus3 实例
date: 2017-12-15 12:41:26
tags: [Docker]
---



## 运行

运行 Nexus3 时候一般会暴露出指定端口绑定到主机；

```
docker run -d -p 8081:8081 --name nexus sonatype/nexus3
```

测试：

```
# 浏览器中访问地址
localhost:8081

# curl 检测地址

curl -u admin:admin123 http://localhost:8081/service/metrics/ping

```

## 说明

+ 默认凭证是： admin/admin123
+ 持久数据推荐方法：

    ```
    $ docker volume create --name nexus-data
$ docker run -d -p 8081:8081 --name nexus -v nexus-data:/nexus-data sonatype/nexus3
```


## 搭建私服

浏览器中输入`localhost:8081` 进入 Nexus 平台

1. 登录系统

    输入用户名与密码 admin/admin123
    ![](http://p57e5dawm.bkt.clouddn.com/login.png)



2. 创新私有库

    按照步骤操作
![](http://p57e5dawm.bkt.clouddn.com/create.png)

    第三部后选择 `docker(hosted)`
    
    填写 name 按照下图创建
    
    ![](http://p57e5dawm.bkt.clouddn.com/docker-hosted.png)

    最后点击 `Create repository`


## 使用

如何使用可以看我之前文章 [上传库到私服](http://chunyang1988.com/2017/11/28/nexus-upload/)

