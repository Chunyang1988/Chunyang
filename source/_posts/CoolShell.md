---
title: 炫酷终端
tags: [Shell]
categories: [Tools]
date: 2017-08-03 15:14:41
---

此文讲解的是通过[oh_my_zsh](http://ohmyz.sh/)来打造个性界面。
建议Mac系统使用[Iterm2](https://www.iterm2.com/) + [oh_my_zsh](http://ohmyz.sh/)进行打造炫酷终端。

## Installation 安装

根据需求进行如下操作，如在终端中可以直接输入

```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```
即可安装。

via curl

```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

via wget

```
sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
```


安装完成后，在用户目录有个隐藏文件夹`.oh-my-zsh`可以使用命令行查看

```
cd ~/.oh-my-zsh
```

## Updates 升级

默认情况下，系统会每隔几周检测升级，发现更新后提示用户进行更新。当然我们也可以关闭提示自动升级，还可以直接关闭自动升级功能。

* 开关提示升级
`DISABLE_UPDATE_PROMPT = true`
* 开关自动升级
`DISABLE_AUTO_UPDATE = true`

对`~/.zshrc`进行编辑。建议使用vi或者vim进行编辑更改。

手动升级
在终端中输入`upgrade_oh_my_zsh`

## Edit Themes 更换主题

所有主题都在`~/.oh-my-zsh/themes`目录中，想要预览可以看[官网地址](https://github.com/robbyrussell/oh-my-zsh/wiki/Themes)

更换主题以及操作升级开关，都是在`~/.zshrc`中进行编辑。

使用vi或者vim对`~/.zshrc`进行编辑 `ZSH_THEME="robbyrussell"`等于号后面写上你要替换的主题名称即可。例如`ZSH_THEME="af-magic"`。

## Uninstall 卸载

卸载oh_my_zsh只需要运行`uninstall_oh_my_zsh`即可。

