---
layout:       post
title:        "Windows 下在命令行编译 CUDA 文件"
subtitle:     "Windows 下脱离 Visual Studio IDE，在命令行下编译 CUDA 文件"
date:         2019-01-20 22:52:00
author:       "LiuTong"
header-img:   "img/in-post/post-bg-nvcc-cu-file-in-console-in-windows.png"
header-mask:  0.3
catalog:      true
tags:
    - CUDA
---

## 运行环境
* Windows 版本：win10 1803 x64
* CUDA 版本：cuda 10.0
* C/C++ 编译器：VC
* Visual Studio 版本：VS2017 community

## 注意事项
CUDA 10.0 对于操作系统、主机和 Visual Studio 位数有严格要求，对于原生 x86/x64 或是交叉 x86/x64，其支持性是不一样的，具体支持情况可去 NVIDIA 官网查询 CUDA Toolkit Documentation。 对于现代编程环境来说（现如今是2018年），基本上都是原生 x64，我的电脑亦如此，主机和操作系统都是原生 x64。

我于 2018 年 11 月开始使用 CUDA，经典资料 《CUDA BY EXAMPLE》 的中文译文版已经是 2010 年出版的了，这 8 年间 CUDA 的发展很迅速，所以这本资料基本上属于很老的书了。这本书中的源代码 README 说明中告诉我们安装好 CUDA 后，直接在命令行使用 `nvcc cuda_file.cu` 即可直接在命令行编译 .cu 文件，实际上，由于我使用 Windows 环境编程，在今年（2018年）NVIDIA 官网介绍的 Windows 下安装 CUDA 的过程，是一套（CUDA + Microsoft Visual Studio）完整的流程，整个环境安装完成后直接在 VS2017 中按照它的提示建立 CUDA 项目即可，如果这样的话，整个项目的复杂度一下子提升了，并且无法从底层熟悉 CUDA 文件的编写、编译流程，不利于学习，如果这样的话，《CUDA BY EXAMPLE》中的代码也需要重新在 Visual Studio 项目中重构。为了直接在命令行中执行 .cu 文件并生成可执行文件，我做了如下的探索并获得成功。

## 配置过程

### 将 Visual Studio 的 VC 编译器放在环境变量中

可能这并不复杂，但要注意的是编译器是用于 x64 的还是 x86 的，如果配置不当，`nvcc` 指令无法正常运行。对于我的原生 x64 位系统而言，我要将用于 x64 的 VC 编译器放在环境变量中。

##### 第一步：在系统变量无名称变量 Path 列表中添加如下 2 个位置

* `C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\VC\Tools\MSVC\14.16.27023\bin\Hostx64\x64`
* `C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\Common7\IDE`

##### 第二步：在系统变量中新建一个变量起名为 LIB，为其添加 3 个位置

* `C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\VC\Tools\MSVC\14.16.27023\lib\x64`
* `C:\Program Files (x86)\Windows Kits\10\Lib\10.0.15063.0\ucrt\x64`
* `C:\Program Files (x86)\Windows Kits\10\Lib\10.0.15063.0\um\x64`

##### 第三步：在系统变量中新建一个变量起名为 INCLUDE，为其添加 2 个位置

* `C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\VC\Tools\MSVC\14.16.27023\include`
* `C:\Program Files (x86)\Windows Kits\10\Include\10.0.15063.0\ucrt`

原则只有一个，既然是原生 x64，各个位置一定要选 x64，否则 `nvcc` 指令不会正确的生效。其实以上三个步骤的作用其实是让 VC 编译器脱离 Visual Studio IDE 环境生效，也就是说可以让 VC 编译器 cl.exe 在命令行下编译 .cpp 或 .c 文件。因为 nvcc 编译器虽说是 .cu 的编译器，但是它还是要调用 VC 编译器 cl.exe 来对 .cu 文件进行编译，这也就是说为什么 CUDA 离不开 Visual Studio，而在其他平台上，比如 Linux，可能 CUDA 需要调用 gcc 或 g++ 编译器来完成对 .cu 文件的编译（这只是我的猜测，我还没有真正试验过）。

## 结语

经过以上过程，nvcc 指令就可以正确的生效了，当你在任何一个位置写了一个 .cu 文件，然后打开 CMD，使用 `nvcc` 就可以对它进行编译并生成一个可执行 exe 文件。从而就可以正确的执行《CUDA BY EXAMPLE》中的源代码了。