---
layout: post
title: "PCL+Kinect配置小结(Win7x64+VS2010+all-in-one)"
category: other
tags: [Kinect,notes,视觉]
image:
  feature: article.jpg
comments: True
share: true

---



PCL官网提供的[all-In-one](http://pointclouds.org/downloads/windows.html)安装包。

###第一次尝试：

环境：64位win7，已经安装了**kinect for windows driver**，VS2010 x86，已经删除OpenNI，OpenNI2

安装[all-In-one](http://pointclouds.org/downloads/windows.html)后，出现两个问题：

- 缺少`OpenNI64.dll`。下载放入bin/解决问题，可以启动demo
- 所有demo崩溃，错误模块`msvcr100.dll`，无法解决（解决方案）
- OpenNI自带例程无法读取kinect（all-in-one不包含kinect驱动）

###第二次尝试
删除了**kinect for windows driver**
首先安装[all-In-one](http://pointclouds.org/downloads/windows.html)，然后卸载all-in-one自带的OpenNI，并安装如下4个包。（按照如下顺序安装）

1. `OpenNI-Win64-1.5.4-Dev`
2. `nite-win64-1.5.2.21-dev` 
3. `SensorKinect093-Bin-Win64-v5.1.2.1`
4. `sensor-win64-5.1.2.1-redist`

安装完成后测试OpenNI没有问题，可以读出kinect的数据，但是所有demo仍然crash。

###第三次尝试
重启电脑，用VS2010x86安装包修复VS2010，测试OK。安装成功

**PS：非常感谢PCL点云2群朋友们的帮助，尤其是Randy**
