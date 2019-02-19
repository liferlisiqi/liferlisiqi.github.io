---
layout:     post
title:      在Android环境使用原生OpenCV         
date:       2019-02-19   
author:     lsq    
header-img: img/post-bg-coffee.jpeg
catalog: true
tags:
    - ANDROID
---

安卓手机端的CPU性能越来越强大，有些手机已经搭配了专用的图像处理芯片，这给在移动端图像处理提供了有利的硬件基础。Android的原本开发语言是Java，（不过现在好像要被Kotlin取代？），Java的性能一般来说不如C/C++，所以直接使用Java进行图像处理不是一个明智的选择。好在现在已经有很多开源的图像处理算法库，其中最常用的OpenCV就是用C/C++进行的底层实现，其中一些算法还针对OpenCL进行了优化。

OpenCV是一个集成了常见图像处理算法的开源库，其包含的许多成熟算法已经在很多场景中有了应用。在Android应用中调用OpenCV的方式，按我的理解分为两种：直接调用OpenCV的Java接口和调用JNI在C++原生函数中调用。个人感觉OpenCV对Java的支持远不如C++，经常出现 native implementation nof found，也就是找不到原生应用的错误。所以配合JNI在C++中调用OpenCV是比较好的选择。

## 1 Android Studio配置OpenCV
AS是Android开发的首选环境，所以这里就介绍针对AS的OpenCV的环境配置。这里有一篇通俗易懂的教程，其中的前六节详细介绍了环境配置，后面内容是使用的Java接口。

[A Beginner’s Guide to Setting up OpenCV Android Library on Android Studio](https://android.jlelse.eu/a-beginners-guide-to-setting-up-opencv-android-library-on-android-studio-19794e220f3c)  

按照前六节操作完成后，我们的环境就配置好了，下面就是如何调用的问题。

## 2 以laplace变换为例说明通过JNI调用OpenCV

我们想通过JNI调用OpenCV的话，采用NDK进行编译的话，至少需要这样以下文件：xxx.cpp, xxx.java, Android.mk, Application.mk，其中java和cpp文件的命名必须一致。java文件中是函数的声明，cpp文件是函数的具体实现，下面是我的程序。

```java
//OpencvHelper.jave

import android.graphics.Bitmap;

public class OpencvHelper {

    public static native float Laplace(Bitmap input);

    static {
        System.loadLibrary("opencv_java3");
        System.loadLibrary("OpencvHelper");
    }
}
```

在上面的java程序中，我们注意到调用了两个库：opencv_java3 和 OpencvHelper。第一个是上一节拷贝过来的，第二个我们一会儿要自己编译生成。下面是对应的cpp文件，在这里我们调用了laplacian变换算法检测图像的模糊程度，实现方式和正常的cpp是一样的，不过需要注意的是函数签名一定要对应好。

```c++
//OpencvHelper.cpp

#include <jni.h>
#include <opencv2/opencv.hpp>
#include <opencv2/imgproc.hpp>
#include <android/bitmap.h>

extern "C" {

JNIEXPORT jfloat JNICALL
Java_com_qy_detect_OpencvHelper_Laplace(JNIEnv *env, jobject obj, jobject input){

    AndroidBitmapInfo info;
    AndroidBitmap_getInfo(env, input, &info);
    int width = info.width;
    int height = info.height;
    void* indata;
    AndroidBitmap_lockPixels(env, input, &indata);
    cv::Mat* mat = new cv::Mat(width, height, CV_8UC1, indata);
    cv::Mat* lap = new cv::Mat(width, height, CV_8UC1);

    cv::Laplacian(*mat, *lap, CV_8U);
    cv::Mat mean(1,4,CV_64F),std(1,4,CV_64F);
    cv::meanStdDev(*lap, mean, std);
    AndroidBitmap_unlockPixels(env, input);
    mat->release();
    lap->release();
    float res = std.at<double_t >(0) * std.at<double_t >(0);
    mean.release();
    std.release();
    return (jfloat)res;

}
}
```

接下来就是使用ndk编译OpencvHelper库文件，编译方式很简单，就是cd到jni目录下然后ndk-build。需要配置的文件是 Android.mk，我的配置如下，如果想同时得到多个库，将下面的代码直接加到原来内容的最底下就行。编译完后会在libs文件加出现 libOpencvHelper.so，这就是我们需要的库文件啦！
```shell
include $(CLEAR_VARS)
OPENCV_CAMERA_MODULES:=on
OpenCV_CAMERA_MODULES := off
OPENCV_LIB_TYPE :=STATIC
include /home/lsq/softs/OpenCV-android-sdk/sdk/native/jni/OpenCV.mk
LOCAL_SRC_FILES := OpencvHelper.cpp
LOCAL_MODULE := OpencvHelper
LOCAL_LDLIBS +=  -lm -llog -ljnigraphics
include $(BUILD_SHARED_LIBRARY)
```

完整的代码可以在这里找到：[liferlisiqi/android-object-detect](https://github.com/liferlisiqi/android-object-detect)  

## Reference
[A Beginner’s Guide to Setting up OpenCV Android Library on Android Studio](https://android.jlelse.eu/a-beginners-guide-to-setting-up-opencv-android-library-on-android-studio-19794e220f3c)  
