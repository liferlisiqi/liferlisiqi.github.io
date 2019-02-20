---
layout:     post
title:      MobileNetSSD模型量化及使用NCNN推断         
date:       2019-02-20   
author:     lsq    
header-img: img/post-bg-coffee.jpeg
catalog: true
tags:
    -DeepLearning - ANDROID
---

最近在安卓移动端开发一个可以实时检测车辆行人的app，计算框架采用tencent的ncnn，深度神经网络模型采用MobileNetSSD，这也是现在网上流传比较广的一个例子。受限于移动端计算资源，几乎不可能完成网络模型的实时推断，模型量化的技术也就应运而生。本文将介绍：将一个预训练好的MobileNetSSD(caffe)模型进行量化、量化caffe模型转换成ncnn模型以及MobileNetSSD(ncnn)模型的加载推断。


## 1 MobileNetSSD模型量化

预训练好的caffe模型可以在这个项目下载 [chuanqi305/MobileNet-SSD](https://github.com/chuanqi305/MobileNet-SSD.git) ， 把项目clone下来后，我们需要其中的：deploy.prototxt、train.prototxt 和 MobileNetSSD_deploy.caffemodel。有的地方说deploy.prototxt这个文件在后面会出现问题，然后给出了另一个.prototxt，[MobileNetSSD_deploy.prototxt](https://download.csdn.net/download/qq_33431368/10850770)。

由于ncnn只能支持最新版本的caffe模型进行转换，所以在做量化之前我们需要先将caffe模型进行upgrade。升级模型的工具caffe已经帮我们做好了，就在 path_to_caffe/build/tools 中。这里有一个很关键的点需要注意，关于ssd相关模型的操作，需要使用caffe/ssd分支，[weiliu89/caffe](https://github.com/weiliu89/caffe/tree/ssd)。编译好caffe后，就可以找到升级caffe模型了，具体教程参考 这里 [ncnn组件使用指北alexnet](https://github.com/Tencent/ncnn/wiki/ncnn-%E7%BB%84%E4%BB%B6%E4%BD%BF%E7%94%A8%E6%8C%87%E5%8C%97-alexnet)。

得到了新版caffe模型后，我们就可以进行量化了，量化工具也早就有人做好了，开心。在 [BUG1989/caffe-int8-convert-tools](https://github.com/BUG1989/caffe-int8-convert-tools) 项目的 README 中已经介绍如何量化模型的方法，这里说下其中一些需要配置的参数。--mean 和 --norm 可以在 train.prototxt 中找到，分别对应 mean_value 和 scale，量化数据集 --images 我用的是PASCAL 2012数据集，这个就根据个人爱好吧。

一切就绪，只欠终端。将下面这条命令输入终端然后回车，不出意外的话，肯定不会顺利执行的，我遇到的问题是caffe路径。

```shell
python caffe-int8-convert-tool-dev.py --proto=../deploy_new.prototxt --model=../deploy_new.caffemodel --mean mean_value mean_value mean_value --norm=scale --images=images/ output=deploy_new_int8.table --gpu=0
```

记得上面提到的caffe/ssd，这里python调用的一定也是这个。解决方法是在 import caffe 之前加上下面两条语句来配置caffe路径，然后在重新执行上面的命令即可。

```python
caffe_root = "path_to_caffe(ssd)"
sys.path.insert(0, os.path.join(caffe_root, 'python'))
```

我使用cpu加上2000多张图片进行的量化，大约花费了不到十分钟的样子，完成后得到了 deploy_new_int8.table ，大小是2.7K，到这里MobileNetSSD模型量化就完成了，鼓掌！！！




## 2 caffe模型转换ncnn模型


## 3 MobileNetSSD使用NCNN推断



## Reference
[chuanqi305/MobileNet-SSD](https://github.com/chuanqi305/MobileNet-SSD.git)  
[weiliu89/caffe](https://github.com/weiliu89/caffe/tree/ssd)  
[Tencent/ncnn/wiki/quantized-int8-inference](https://github.com/Tencent/ncnn/wiki/quantized-int8-inference#caffe-int8-convert-tools)  
[Tencent/ncnn/wiki/ncnn组件使用指北alexnet](https://github.com/Tencent/ncnn/wiki/ncnn-%E7%BB%84%E4%BB%B6%E4%BD%BF%E7%94%A8%E6%8C%87%E5%8C%97-alexnet)  
[BUG1989/caffe-int8-convert-tools](https://github.com/BUG1989/caffe-int8-convert-tools)  
[]()  
[]()  
[]()  
