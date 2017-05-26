---
layout: post
title: 使用Arch Linux时出现的问题
date: 2017-05-19 15:32:24.000000000 +09:00
category: Linux
tag: Linux 
---

使用了Arch快半年了，这个文章实在是写晚了。。

---

### oh my zsh 安装使用
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

### error: failed to commit transaction (conflicting files)
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

### 窗口撕裂

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


### signature from xxx is marginal trust


> example:

> error: archlinuxcn-keyring: signature from "Jiachen Yang \<farseerfc@gmail.com />" is marginal trust
> :: File /var/cache/pacman/pkg/archlinuxcn-keyring-20170522-1-any.pkg.tar.xz is corrupted (invalid or corrupted package (PGP signature)).

**解决办法**

`sudo pacman-key --refresh-keys`

代码运行结果就是新导入了很多PGP signature, 然后问题解决。

**我不知道这是不是唯一的解决方法，还需要进一步完善。**

**解决方法二**

```shell
sudo rm -R /etc/pacman.d/gnupg/
pacman-key --init
pacman-key --populate archlinux
pacman-key --populate archlinuxcn
```
可以看出这也是走的更新key的路数。

但是出现这种问题执行`pacman -S archlinux-keyring`重装`archlinux-keyring`是没有用的。

***还有一种说法是系统时间不正确***

不知道这个系统时间是指哪个时间。

### /opt sysmlink file conflicts 

由于在安装Arch系统时，/home分区是独立于/root分区的，而yaourt安装的大型软件都是会安装到/opt下面，导致系统分区会被很快用光。所以我的解决方法是在/home分区下新建opt分区，然后建立软链接：

`ln -s /home/opt /opt`

但是这样做之后会导致在yaourt安装软件是出现`/opt already exists`的`file conflicts`错误。

官方解释如下：

> Directory Symlink Handling: Example time! Arch Linux has a /lib -> /usr/lib symlink. Previously, if pacman was installing a package and it found files in /lib, it would follow the symlink and install it in /usr/lib. However the filelist for that package still recorded the file in /lib. This caused heaps of difficulty in conflict resolving – primarily the need to resolve every path of all package files to look for conflicts. That was a stupid idea! So now if pacman sees a /lib directory in a package, it will detect a conflict with the symlink on the filesystem. If you were using this feature to install files elsewhere, you probably need to look into what a bind mount is! Note that this change requires us to correct the local package file list for any package installed using this mis-feature, so we bumped the database version. Upgrade using pacman-db-upgrade. Thanks to Andrew! 

解决方法如下：

在**/etc/fstab**里加入如下内容：

```shell
# solve the /opt sysbolink file conficts errors
/home/opt	/opt	none	bind
```
**problems solved, splendid!**
 
