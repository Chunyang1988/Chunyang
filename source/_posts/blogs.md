---
title: "搭建个人博客"
date: 2017-08-01 16:46:50
tags:
---

此文讲解的搭建个人博客是通过[GitHub Pages](https://pages.github.com/) + [Hexo](https://hexo.io/zh-cn/)进行搭建。

## 1.安装Hexo

安装前需要需要确定是否安装[Node.js](https://nodejs.org/en/)以及[Git](https://git-scm.com/)，在此文我会重头一步一步安装操作。

#### a.安装 Homebrew
Homebrew是 macOS 缺失的软件包管理器，具体哪些好处问什么要安装还是自行搜索吧。

安装只需要在终端中输入:

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
安装成功后，直接就可以使用brew进行安装Node以及Git了，当然可以安装的还有很多东西，就不列举了。

#### b.安装Node

安装官网安装Node也可以，但是此文讲解的是通过Brew进行安装，大家一起看看Homebrew的优点。

同样只需要在终端中输入:

```
brew install node
```
即可，这就是软件管理器的优点之一了。

#### c.安装Git

此处就不多说了直接上代码

```
brew install git
```

#### d.安装Hexo
至此准备工作都已经完成可以，开始安装Hexo

```
 npm install -g hexo-cli
```
    
## 2.建站

安装完成Hexo后，运行一下命令进行建站操作。

```
hexo init <folder>
cd <folder>
npm install
```

并不一定非要按照上述代码去写，也可以这样写

```
mkdir <folder>
cd <folder>
hexo init
npm install
```
例如：

```
mkdir Blogs
cd Blogs
hexo init 
npm install
```

完成后目录结构如下

```
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```
具体什么意思，这里面就不废话了，具体可以看[官网
](https://hexo.io/zh-cn/docs/setup.html)

## 3.简单写作

此处不会过度介绍，具体可以看官网，只会简单介绍几种常用命令。

#### a.新建文章

```
hexo new [layout] <title>
```
其中`[layout]`有三种默认布局：`post`、`page`、`draft`，分别对应不同路径。

| 布局 | 路径 |
| --- | --- |
| post | source/_posts |
| page | source |
| draft | source/_drafts |

例如：

```
hexo new blogs
```

#### b.更改主题

使用Nlvi主题为介绍

```
git clone https://github.com/tufu9441/maupassant-hexo.git themes/maupassant
npm install hexo-renderer-jade@0.3.0 --save
npm install hexo-renderer-sass --save
```
将主题下载到指定目录，更改配置文件`_config.ym`里面的theme

```
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: maupassant
```
具体更改说明可以看官网详细说明。

## 4.部署
#### a.创建Github Pages

这里面不多说，主要是创建一个yourName.github.io仓库

#### b.部署Hexo到Github

这里面主要介绍使用hexo deploy部署到git上面

在配置文件`_config.yml`中配置deploy

```
deploy:
  type: git
  repo: git@github.com:yuserName/yuserName.github.io.git
  branch: master
```

例如：

```
deploy:
  type: git
  repo: git@github.com:Chunyang1988/chunyang1988.github.io.git
  branch: master
```

之后一件部署

```
hexo deploy
```
#### c.添加SSH密钥

生成密钥

```
ssh-keygen -t rsa -C "your_email@example.com"
```
例如

```
ssh-keygen -t rsa -C "chunyang1988.cn@gmail.com"
```
为了方便在后面提示信息中，直接按回车，一直回车下来。

#### d.部署

部署前还需要安装

```
npm install hexo-deployer-git --save
```

一般现在本地写好文章，运行

```
hexo s
```
本地查看一下，如果可以，可以直接部署到git上面

```
hexo d
```

## 结束

这时候你输入自己的xxx.github.io即可查看自己的博客了。



