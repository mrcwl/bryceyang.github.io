---
layout: post
title: 使用Arch Linux时出现的问题
date: 2016-02-16 15:32:24.000000000 +09:00
---

使用了Arch快半年了，这个文章实在是写晚了。。
#### oh my zsh 安装使用
* 首先安装Zsh *
``` shell
sudo pacman -S zsh
```
* 安装oh my zsh *
```
sh -c "$(wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
```
* 切换shell从bash到zsh *
首先查看可用shell列表：
``` shell

➜  ~  cat /etc/shells
#
# /etc/shells
#

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


