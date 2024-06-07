---
layout: post
title:  "关于Linux 软件包"
author: XM137
date:   2024-06-07 11:26:00 +0800
categories: Daily
tags: Linux Package deb rpm appimage
---

## 软件包
叫安装包或许更通俗，一般来说，其中包含一些基本的文件，可执行文件，库文件，等等

## deb 软件包
deb是debian系统的安装包，Ubuntu，Kali等都基于Debian，使用deb软件包

除基本程序文件外，还有描述文件，其中包含软件包名称，介绍，依赖，版本，架构，等等

rpm如CentOS，RHEL都会使用rpm包

rpm 软件包，与之类似，但是不完全相同

## AppImage
严格意义上来说它不是软件包，而是一个完整的可执行的文件，它包含所需依赖库，无需解包安装，即可直接运行

pacman你会在Arch Linux上见到
pacman软件包管理器所安装的软件包，可以是xz也可以是tar，总之就是一种压缩格式，各家软件包格式虽说有所不同，文件目录不同之外，其他的都大多相似，基本程序文件+描述文件+其他的一些文件，在此不再一一列举，简单了解即可

呃....

如有错误，待更...

