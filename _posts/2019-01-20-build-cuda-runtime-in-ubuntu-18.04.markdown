---
layout:       post
title:        "Ubuntu18.04 下搭建 CUDA 环境并安装 NVIDIA 显卡驱动"
subtitle:     "在 Ubuntu18.04 下安装 CUDA 并为 CUDA 安装匹配的显卡驱动"
date:         2019-01-20 23:08:00
author:       "LiuTong"
header-img:   "img/in-post/post-bg-build-cuda-runtime-in-ubuntu-18.04.jpg"
header-mask:  0.3
catalog:      true
multilingual: false
tags:
    - Linux
    - CUDA
---

> 世上无难事，只要肯登攀。
>             ------ 毛泽东

## 前言
说句实话，在 Linux 环境下搭建 CUDA 环境对我来说简直是一种折磨，其实在 Liunx 下安装 CUDA，其过程本身并不复杂，难度等级和 Windows 上的 CUDA 环境搭建差不多是一个量级的，甚至还简单一些，但是在 Liunx 上安装 NVIDIA 显卡驱动的过程可谓惊心动魄，每一步都有将系统搞崩溃的可能（一点也不夸张），最可气的是网上没有一篇真正靠谱的教程，你弄不清哪一篇教程是真正可以操作的，但是说来也情有可原，因为千人千面，每个人在遇到这个问题时都很焦急，都是病急乱投医，每个人遇到的错误也不一样，这就导致会出现以上情况。也许你在看我这篇教程的时候也会觉得不尽如人意，因为你出现问题的系统环境和遇到的问题可能与我又不一样，但是有教程总比没有强，有一点参考性的东西可能会带来不一样的效果，所以这里就将我在 Ubuntu18.04 环境下搭建 CUDA 的艰辛过程总结一下，由于当时很多操作在命令行模式下进行的，所以不能截图，另一方面由于比较着急，所以也没有心情拍照，所以这里就凭借记忆来将这个过程总结一下，尽量争取详实一些。

另，百度的搜索水平我也是佛了。

## 环境
* 操作系统：Ubuntu 18.04.1 LTS Desktop
* CUDA 版本：CUDA 10.0.30
* NVIDIA 显卡：NVIDIA GT630

## 安装 NVIDIA 显卡驱动程序
要添加 NVIDIA 显卡驱动程序，在 Liunx 下有好几种方式，其中有一种最简单的方式，如下：

首先执行命令：

```shell
ubuntu-drivers devices
```

然后会查询到电脑的显卡信息，显示如下：

```shell
== /sys/devices/pci0000:00/0000:00:01.0/0000:01:00.0 ==
modalias : pci:v000010DEd00000FC2sv00001462sd00002752bc03sc00i00
vendor   : NVIDIA Corporation
model    : GK107 [GeForce GT 630 OEM]
manual_install: True
driver   : nvidia-340 - distro non-free
driver   : nvidia-driver-390 - distro non-free recommended
driver   : xserver-xorg-video-nouveau - distro free builtin
```

其中下面标示 driver 的几行表示对你这个型号的显卡支持的驱动程序版本，其中第二个 driver，也就是 nvidia-driver-390 后面标示着 recommended，也就是这是推荐的型号，接下来你就可以使用如下的命令安装这个推荐的驱动程序：

```shell
sudo ubuntu-drivers autoinstall
```

之后驱动程序就会自动安装，安装完成后你执行如下的命令就可以查询到显卡驱动程序的信息：

```shell
cat /proc/driver/nvidia/version
```

如果顺利的话，就会看到如下的信息
```shell
NVRM version: NVIDIA UNIX x86_64 Kernel Module  410.93  Thu Dec 20 17:01:16 CST 2018
GCC version:  gcc version 7.3.0 (Ubuntu 7.3.0-27ubuntu1~18.04)
```