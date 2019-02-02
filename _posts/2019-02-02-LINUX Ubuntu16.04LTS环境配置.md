---
layout:     post
title:      Ubuntu16.04LTS环境配置         
date:       2019-02-02   
author:     lsq    
header-img: img/post-bg-coffee.jpeg
catalog: true
tags:
    - LINUX
---

不管是虚拟机还是双系统还是单系统，每次装完系统后都不可避免的一顿配置环境，这个过程中又是一顿谷歌。所以，是时候把一些经常用到的软件和工具总结一下。

## 1 装机必备

首先是装完Ubuntu系统后，必须(墙裂建议)进行的安装操作。


### 1.1 软件源设置
软件软设置的目的在于以后装软件的时候可以速度快一点，设置方法是：setting->software & updates->ubuntu software->download from->others->China->这里选一个你喜欢的就好，我选的是清华源 https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ ，上不了贵清用用你们的资源总可以吧。


### 1.2 shadowsocks配置
在进行一切操作之前，建议最先进行的操作是翻墙，没有之一。之后可能会出现的因为下载源在国外的问题，都将不复存在XDDD。我翻墙的方式是使用shadowsocks，至于如何搭梯子是不属于linux环境配置范围，所以这里直接使用可以翻墙的梯子。

安装 Shadowsocks 的教程参考这篇：[大宝日记/ubuntu使用shadowsocks](https://www.sundabao.com/ubuntu%E4%BD%BF%E7%94%A8shadowsocks/)。我使用的是第二种方法，就是带GUI界面的ss，因为只需要如下三行命令就可以完成。

```shell
sudo add-apt-repository ppa:hzwhuang/ss-qt5
sudo apt-get update
sudo apt-get install shadowsocks-qt5
```

之后就可以在启动项中找到Shadowsocks了，就是下面的界面。点击 connection->add->manually 后出现ss配置界面，现在把梯子搭在这里就好了。到这里我们本地的ss就配置好了，之后需要在浏览器(chrome/firefox)再进行一些配置，方法依旧参考上面大宝的教程即可。

![](https://raw.githubusercontent.com/liferlisiqi/liferlisiqi.github.io/master/img/2019-02-02-linux1.png)

### 1.3 Chrome安装及配置
作为一个常年在各种设备上使用Chrome的用户，面对一个崭新的系统，当然迫不及待的想给它装个Chrome。这个方法有很多，下面这三条命令属于狠话不多的一种，一般情况下都可以顺利的装上。
```sh
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
sudo dpkg -i google-chrome-stable_current_amd64.deb
sudo apt-get install -f
```

### 1.4 显卡驱动
使用Ubuntu到目前为止，发现需要人工安装的驱动只有显卡驱动。安装显卡驱动有两个方法：系统适配显卡驱动和通过命令行安装，其中第一个方法十分的简单粗暴。下面两篇教程可以作为参考。

[Ubuntu16.04安装NVIDIA（GeForce1080Ti）显卡驱动](https://blog.csdn.net/QLULIBIN/article/details/79947062)
[ubuntu16.04下NVIDIA GTX965M显卡驱动PPA安装](https://blog.csdn.net/10km/article/details/61191230)




## reference

[大宝日记/ubuntu使用shadowsocks](https://www.sundabao.com/ubuntu%E4%BD%BF%E7%94%A8shadowsocks/)
