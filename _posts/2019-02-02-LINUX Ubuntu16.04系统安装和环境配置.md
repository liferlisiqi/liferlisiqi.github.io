---
layout:     post
title:      使用U盘安装Ubuntu16.04LTS系统        
date:       2019-02-02   
author:     lsq    
header-img: img/post-bg-coffee.jpeg
catalog: true
tags:
    - LINUX
---

前两天拿到一台新电脑，抬手我就想装一个Ubuntu，那成想装了一下午才装好。现在回顾一下装机过程和遇到的一些问题，算是给自己做个记录。这次安装Ubuntu是在裸机上进行安装，不涉及到双系统问题。目前为止，Ubuntu16.04LTS版本是比较常用且较为稳定的版本，对很多开发环境支持的都很好，所以选择该版本进行安装。安装Ubuntu16.04LTS分为两个步骤：制作U盘启动盘和安装Ubuntu16.04


## 1 制作U盘启动盘

使用U盘进行Ubuntu安装是最常规的操作，也是比较容易搞定的，制作启动盘的时候我们需要一个大于8GB的U盘和Ubuntu镜像文件，制作工具最好使用 Universal USB Installer， 我之前使用其他工具的时候会出现启动盘无法启动的问题。

Ubuntu16.04的镜像文件从官网下载就好： [下载Ubuntu桌面版](http://www.ubuntu.org.cn/download/desktop)，一般我们个人使用的都是64位的桌面版，所以不要下载服务器版本。还有就是对于新手来说，最好下载带有LTS后缀的镜像，因为这代表该镜像是长期维护的，相对来说更为稳定。现在就可以制作启动盘了，下面是官网的教程,分别给出在三个系统下的制作方法，再也不用担心不会制作启动盘了。官网推荐的工具是Rufus，我还没有用过，等有机会尝试以下。

[Create a bootable USB stick on Windows](https://tutorials.ubuntu.com/tutorial/tutorial-create-a-usb-stick-on-windows#9)  
[Create a bootable USB stick on Ubuntu](https://tutorials.ubuntu.com/tutorial/tutorial-create-a-usb-stick-on-ubuntu#0)  
[Create a bootable USB stick on macOS](https://tutorials.ubuntu.com/tutorial/tutorial-create-a-usb-stick-on-macos#0)  

这里还有专门使用 Universal USB Installer 制作启动盘的教程：  

[使用Universal USB Installer创建安装Linux U盘系统](https://blog.csdn.net/gongxifacai_believe/article/details/52463126)  
[linux之universal usb installer安装ubuntu](https://blog.csdn.net/u011068702/article/details/52096344)  

## 2 安装Ubuntu16.04

制作好启动盘后我们就可以愉快的安装Ubuntu系统了，不同的电脑如何选择从U盘启动的快捷键不同，这里请自己谷歌，下面依然先给出Ubuntu官网的安装教程：  

[Install Ubuntu desktop](https://tutorials.ubuntu.com/tutorial/tutorial-install-ubuntu-desktop#0)  
[Install Ubuntu 16.04 desktop](https://tutorials.ubuntu.com/tutorial/tutorial-install-ubuntu-desktop-1604#0)  

需要注意的几点在这里：
- 1 既然都用上Ubuntu了，最开始选择语言的时候就选英语吧。如果选择了Chinese，会有一些中文路径，在命令行切换输入法太难受。
- 2 在 `Preparing to install Ubuntu` 界面的两个选项最好不要选，因为安装速度会非常慢，等系统装好后再更新也来的及。
- 3 在 `installation type` 步骤选择 `Something else` ,感觉大多数人装不上就是卡在这里，也就是对系统进行分区(partition scheme)，下面重点说一下这里的操作。

由于当时没有截图，所以只好从网上找了别人的图，界面如下所示。不同于那些妖艳的分区方案，这里建议最好只分两个区，挂载点分别是：/ 和 swap area。为啥这么建议呢，因为不少人选择四个分区后，会出现启动时卡在黑屏光标闪烁，这是因为启动项的问题，我就在这里卡住不止一次。至于两个挂载点的大小，swap建议和内存一样大， / 就是剩下的空间了，如何进行分区可以参考下面的那个教程。分好区后就一路next开始安装，正常安装时间也就二三十分钟。装完会重启，这时拔掉U盘重启，如果顺利的话，就可以成功启动了。

![](https://raw.githubusercontent.com/liferlisiqi/liferlisiqi.github.io/master/img/create-new-partition-table-ubuntu-16-installation.jpg)  

[Steps to Install Ubuntu 16.04 LTS (Xenial Xerus) with Screenshots](https://www.linuxtechi.com/install-ubuntu-16-04-with-screenshots/)  




## Reference
[Create a bootable USB stick on Windows](https://tutorials.ubuntu.com/tutorial/tutorial-create-a-usb-stick-on-windows#9)  
[Create a bootable USB stick on Ubuntu](https://tutorials.ubuntu.com/tutorial/tutorial-create-a-usb-stick-on-ubuntu#0)  
[Create a bootable USB stick on macOS](https://tutorials.ubuntu.com/tutorial/tutorial-create-a-usb-stick-on-macos#0)  
[使用Universal USB Installer创建安装Linux U盘系统](https://blog.csdn.net/gongxifacai_believe/article/details/52463126)  
[linux之universal usb installer安装ubuntu](https://blog.csdn.net/u011068702/article/details/52096344)  
[Install Ubuntu desktop](https://tutorials.ubuntu.com/tutorial/tutorial-install-ubuntu-desktop#0)  
[Install Ubuntu 16.04 desktop](https://tutorials.ubuntu.com/tutorial/tutorial-install-ubuntu-desktop-1604#0)  
[Steps to Install Ubuntu 16.04 LTS (Xenial Xerus) with Screenshots](https://www.linuxtechi.com/install-ubuntu-16-04-with-screenshots/)  
