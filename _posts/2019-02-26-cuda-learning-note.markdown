---
layout:       post
title:        "CUDA 学习笔记 01"
subtitle:     ""
date:         2019-02-26 22:44:00
author:       "LiuTong"
header-img:   "img/in-post/post-bg-nvcc-cu-file-in-console-in-windows.png"
header-mask:  0.3
catalog:      true
tags:
    - CUDA
---

### CUDA 的两层 API

CUDA 提供了两层 API 来管理 GPU 设备和组织线程，分别是

* CUDA 驱动 API
* CUDA 运行时 API

驱动 API 是一种低级 API，它相对来说较难编程，但是它对于在 GPU 设备使用上提供了更多的控制。运行时 API 是一个高级 API，它在驱动 API 的上层实现。每个运行时 API 函数都被分解为更多传给驱动 API 的基本运算。

驱动 API 和运行时 API 之间没有明显的性能差异。在设备端，内核是如何使用内存以及你是如何组织线程的，对性能有更显著的影响。

这两种 API 是相互排斥的，你必须使用两者之一，从两者中混合函数调用是不可能的。在接下来的学习中，全部使用运行时 API。

### 第二