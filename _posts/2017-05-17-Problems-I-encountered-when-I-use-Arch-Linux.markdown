---
layout: post
title: 使用Arch Linux时出现的问题
date: 2017-05-19 15:32:24.000000000 +09:00
category: Linux
tag: Linux 
---

使用了Arch快半年了，这个文章实在是写晚了。。
#### oh my zsh 安装使用
**首先安装Zsh**

``` shell
sudo pacman -S zsh
```

**安装oh my zsh**

```
sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
```

**切换shell从bash到zsh**

首先查看可用shell列表：

``` shell
➜  ~  cat /etc/shells
# /etc/shells
/bin/sh
/bin/bash
# End of file
/bin/zsh
/usr/bin/zsh
```

切换到zsh

``` shell
chsh -s /usr/bin/zsh
```
#### error: failed to commit transaction (conflicting files)
这个问题已经碰到了几次，原因暂未知。

``` shell
error: failed to commit transaction (conflicting files)
python-markupsafe: /usr/lib/python3.6/site-packages/MarkupSafe-1.0-py3.6.egg-info/PKG-INFO exists in filesystem
python-markupsafe: /usr/lib/python3.6/site-packages/MarkupSafe-1.0-py3.6.egg-info/SOURCES.txt exists in filesystem
python-markupsafe: /usr/lib/python3.6/site-packages/MarkupSafe-1.0-py3.6.egg-info/dependency_links.txt exists in filesystem
```
** 解决方法 **
首先，检测一下冲突文件有没有包在使用
```
pacman -Qo /path/to/file
```
结果一般就是没有包在使用。。所以下面就rm掉所有引起冲突的文件就行了，问题解决。

