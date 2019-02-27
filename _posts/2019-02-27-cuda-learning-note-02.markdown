---
layout:       post
title:        "CUDA 学习笔记 02"
subtitle:     "CUDA 编程模型"
date:         2019-02-27 23:24:00
author:       "LiuTong"
header-img:   "img/in-post/post-bg-nvcc-cu-file-in-console-in-windows.png"
header-mask:  0.3
catalog:      true
tags:
    - CUDA
---

从 CUDA 6.0 开始，NVIDIA 提出了名为“统一寻址”（Unified Memory）的编程模型的改进，它连接了主机内存和设备内存空间，可以使用单个指针访问 CPU 和 GPU 内存，无须彼此之间手动拷贝数据。而现在，重要的是需要学会如何为主机和设备分配内存空间以及如何在 CPU 和 GPU 之间拷贝共享数据，这种程序员管理模式控制下的内存和数据可以优化应用程序并实现硬件系统利用率的最大化。

## 一般编程模型