---
layout: post
title: 使用Arch Linux时出现的问题
date: 2017-05-19 15:32:24.000000000 +09:00
category: Linux
tag: Linux 
---

使用了Arch快半年了，这个文章实在是写晚了。。

---

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

---

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
结果一般就是没有包在使用。。所以下面就rm掉所有引起冲突的文件就行了，问题解决。最近碰见了这个问题好几次，但是出现原因暂未知。。太烦了。

---

#### 窗口撕裂

我不知道这个问题应不应该叫做窗口撕裂：在上下滚动内容的时候，滚动方向最下面的内容会出现重影，显得好像屏幕刷新有延迟。

**解决方法**

```shell
sudo vim /etc/X11/xorg.conf.d/20-intel.conf
```

然后添加如下内容：

```
Section "Device"
   Identifier  "Intel Graphics"
   Driver      "intel"
   Option      "TearFree" "true"
   Option      "AccelMethod"  "uxa"
EndSection
```

> 默认的`AccelMethod`的值为`sna`，在我这里使用`sna`会出现滚动延迟的情况，但是使用`uxa`就没有这种现象。但是更改了这个值之后发现了一个新问题就是笔记本不合盖子屏幕自动关闭之后不能唤醒，只有切到tty之后再切回来才行。待解决。

