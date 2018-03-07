---
title: Docker 入门介绍
date: 2017-12-03 12:38:26
tags: [Docker]
---


# Image 镜像
Docker 镜像是一个特殊的文件系统，除了提供容器运行时所需的程序、库、资源、配置等文件外，还包含了一些为运行时准备的一些配置参数。

Docker 把应用程序及其依赖，打包在 image 文件里面。

Image 镜像文件是通用的，可以跨机器使用，为了方便共享，image 文件制作完成后，可以上传到仓库中。Docker 的官方仓库 Docker Hub 是最重要、最常用的 image 仓库。

## 基础指令

```
# 列出本机所有的 image 镜像文件
docker image ls 

# 删除 image 镜像文件
docker iange rm [imageName]

# 获取镜像
docker pull [选项] [Docker Registry 地址[:端口号]/]仓库名[:标签]
```

### 获取镜像

```
docker image pull library/hello-world
```

`library`是 image 文件所在的组，而Docker Hub 官网的默认组即`library`因此可以不写；

`hello-world` 则是镜像名称；

### 查询镜像

获取成功后，就可以查看这个 image 镜像

```
docker image ls
```

### 删除镜像

那么如何删除不用的镜像呢？

```
docker image rm [选项] <镜像1> [<镜像2> ...]
```
其中，<镜像> 可以是 镜像短 ID、镜像长 ID、镜像名 或者 镜像摘要。

## container 容器
容器是独立运行的一个或一组应用，以及它们的运行态环境。对应的，虚拟机可以理解为模拟运行的一整套操作系统（提供了运行态环境和其他系统环境）和跑在上面的应用。

### 启动容器

```
docker container run hello-world
```

`docker container run` 命令会从 image 文件中生成一个正在运行的容器实例；

`docker container run` 命令会自动抓取 image 镜像中的功能，如果发现本地没有指定的 image 则会自动去仓库中拉取，因此 `docker image pull ` 并不是必需命令；

### 查询容器

image 文件生成的容器实例，本身也是一个文件，称为容器文件；

一旦容器生成，就会同时存在两个文件： image 文件和容器文件。而且关闭容器并不会删除容器文件，只是容器停止运行而已。

```
# 列出本机正在运行的容器
docker container ls

# 列出本机所有容器，包括终止容器
docker container ls --all
```

### 终止容器

```
docker container stop [containerId]
```

### 删除容器文件

上文说了启动容器会生成一个容器文件，现在将删除这个文件。

```
docker container rm [containerID]
```


## 总结

具体的某个镜像如何使用，其实看 Docker Hub 里面说明已经讲解很明了，例如 [nexus3](https://hub.docker.com/r/sonatype/nexus3/)








