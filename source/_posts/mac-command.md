---
title: Mac 终端常用命令
date: 2018-01-12 16:37:17
tags: [Shell , Mac]
---

**列出文件**

```
ls 参数 目录名

# 例如
ls Documents
```

参数：
-w 显示中文
-l 详细信息
-a 显示全部包含隐藏文件

**显示目录路径**

```
pwd
# 结果
/Users/xx/Documents
```

**转换目录**

```
cd 目录名
# 例如
cd Documents
```

**创建新目录**

```
mkdir 目录名
# 例如
mkdir newDir
```

**删除目录**

```
rmdir 目录名
# 例如
rmdir newDir
```



**拷贝文件**

```
cp 参数 源文件 目标文件
# 例如
cp Download/a.txt Documents/a.txt
cp Download/a.txt Documents
```

**删除文件**

```
rm 参数 文件
# 例如
rm -rf Documents/a.txt
```
参数
-f 强制删除

**移动文件**

```
mv 文件
# 移动文件并以原名移动
mv Download/a.txt Documents
# 移动并以新名存储
mv Download/a.txt Documents/b.txt
```

**查看文件**

```
cat 文件名
# 例如
cat t.txt
```

**显示文件类型**

```
file 文件名
# 例如
file t.txt
```

**查找文件**

```
find path -name "xx"
# 例如 查找当前目录后缀为.txt 的文件
find . -name "*.txt"
```

**时间相关**

```
date 显示系统当前日期和时间
cal 显示日历
```

**File 隐藏文件显示/隐藏**

```
# 显示隐藏文件
defaults write com.apple.finder AppleShowAllFiles -bool true
# 恢复隐藏文件
defaults write com.apple.finder AppleShowAllFiles -bool false 
# 关闭所有 Finder
killall Finder
```








