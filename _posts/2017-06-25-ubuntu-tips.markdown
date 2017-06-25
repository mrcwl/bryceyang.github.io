---
layout: post
title: Ubuntu tips
date: 2017-06-25 16:50:00
catagory: Linux tips
tag: Linux Tips

### **1. 更换pip源为阿里云的镜像源**

编辑pip配置文件:

`vim ~/.pip/pip.conf`

然后写入如下内容：

```
[global]
trusted-host = mirrors.aliyun.com
index-url = https://mirrors.aliyun.con/pypi/simple
```

**如果发现没有对应文件夹以及配置文件，请直接自行创建。**

---

### **2. Ubuntu将默认的python版本换为python3.X**

Ubuntu 16.04自带了python2.7和python3.5，且默认版本为python2.7，最近一直使用python3.×版本所以需讲python默认版本修改为3.5。

可以使用`ls /usr/bin | grep python`来查看目前存在的python版本

然后设置软链接，讲python3.5设为默认：

`sudo ln -s /usr/bin/python3.5 /usr/local/bin/python`

此时python3.5已经成为默认。

![效果](http://oq782gkz3.bkt.clouddn.com/Selection_008.png)

**然后此时需要注意的就是，如果以后主要在3.×的环境下工作，请安装对应3.×的包。**

```
sudo apt install python3-pip
pip3 install package_name
```
