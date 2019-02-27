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

### 简单的配置查询操作

* 检查 nvcc 编译器是否正确安装

    使用命令：

    ```shell
    which nvcc
    ```

    输出如下：

    ```shell
    /usr/local/cuda-10.0/bin/nvcc
    ```

* 检查 GPU 加速卡的安装信息

    使用命令

    ```shell
    ls -l /dev/nv*
    ```

    输出如下

    ```
    crw-rw-rw- 1 root root 195,   0 2月  27 20:44 /dev/nvidia0
    crw-rw-rw- 1 root root 195, 255 2月  27 20:44 /dev/nvidiactl
    crw-rw-rw- 1 root root 195, 254 2月  27 20:44 /dev/nvidia-modeset
    ```

    这代表机器中只安装了一块 GPU 加速卡。

### 编程范式

CUDA 文件中，需要使用修饰符 `__global__` 来表示一个函数将会从 CPU 中调用，然后在 GPU 上执行，这样的函数称为`核函数`，在调用核函数的时候，用类似 `函数名 <<< m, n >>> ()` 的形式来调用核函数。

现在编写一个从 GPU 中打印 Hello World 的程序，程序以并行方式执行，并行打印 10 个 Hello World，代码如下：

```cpp
#include <stdio.h>

__global__ void helloFromGPU (void)
{
    printf("Hello World from GPU!\n");
}

int main(void)
{
    printf("Hello World from CPU!\n"); // hello from CPU

    helloFromGPU <<<1,10>>> (); // hello from GPU
    cudaDeviceReset();
    return 0;
}
```

程序执行结果如下：

```shell
Hello World From CPU
Hello World From GPU
Hello World From GPU
Hello World From GPU
Hello World From GPU
Hello World From GPU
Hello World From GPU
Hello World From GPU
Hello World From GPU
Hello World From GPU
Hello World From GPU
```

与 C++ 程序的混写，在编写 CUDA 程序时，在除了带关键字内核函数里，可以使用 C++ 和 CUDA C 混写的方式，然而在带有关键字的核函数中，不可以使用 C++ 的语法来写程序，只能使用 C 的语法来写程序。例如，上面的输出 Hello World 的程序可以写成如下形式：

```cpp
#include <iostream>
#include <stdio.h>

__global__ void helloFromGPU (void)
{
    printf("Hello World from GPU!\n");
}

int main(void)
{
    using namespace std;
    cout << "Hello World from CPU!" << endl;

    helloFromGPU <<< 1, 10 >>> ();
    cudaDeviceReset();
    return 0;
}
```



