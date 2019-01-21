---
layout:       post
title:        "Ubuntu 18.04 下搭建 CUDA 环境并安装 NVIDIA 显卡驱动"
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
* 操作系统：Ubuntu 18.04.1 LTS Desktop 64bit
* CUDA 版本（需要选择）：CUDA 10.0.130
* NVIDIA 显卡型号：NVIDIA GeForce GT630
* NVIDIA 显卡驱动版本（需要安装，与 CUDA 版本相匹配）：410.93

## 安装 NVIDIA 显卡驱动程序

#### 暴风雨的前兆

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

其中下面标示 driver 的几行表示对你这个型号的显卡支持的驱动程序版本，其中第二个 driver，也就是 nvidia-driver-390 后面标示着 recommended，表示这是推荐的型号，接下来你就可以使用如下的命令安装这个推荐的驱动程序：

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

虽然你的显卡驱动安装成功了，但是这个显卡驱动可能不是那么尽如人意的，因为有可能这与你的 CUDA 版本并不兼容，而我就面临了这样的问题，当我在之后的操作中将 CUDA 安装完成后，在将 CUDA Samples 里面的例子全部 Make 完成后，在运行这些二进制文件的时候，系统提示我：

```shell
CUDA driver version is insufficient for CUDA runtime version
```

即我的显卡驱动版本与 CUDA 运行环境版本不匹配，说白了就是 CUDA 版本太高了，而显卡驱动的版本没有那么高，即虽然可以编译这些 CUDA 文件，但是这些编译好的二进制文件在我的显卡上运行不起来。这就导致了我需要将现有的显卡驱动替换掉，换成一个高版本的显卡驱动。那替换成多高的版本合适呢？这就需要去参考 NVIDIA 官网给出的参数：[点击这里查看][NVIDIA-DRIVER-CUDA]。

[NVIDIA-DRIVER-CUDA]: https://docs.nvidia.com/cuda/cuda-toolkit-release-notes/index.html "NVIDIA-DRIVER-CUDA"

以下是截至 2019 年 1 月 21 日，NVIDIA 官网给出的 CUDA 版本号与显卡驱动版本的匹配情况。

| CUDA Toolkit | Linux x86_64 Driver Version | Windows x86_64 Driver Version |
| --------------------------- | --------- | ---------- |
| CUDA 10.0.130               | >= 410.48 |  >= 411.31 |
| CUDA 9.2 (9.2.148 Update 1) | >= 396.37 |  >= 398.26 |
| CUDA 9.2 (9.2.88)           | >= 396.26 |  >= 397.44 |
| CUDA 9.1 (9.1.85)           | >= 390.46 |  >= 391.29 |
| CUDA 9.0 (9.0.76)           | >= 384.81 |  >= 385.54 |
| CUDA 8.0 (8.0.61 GA2)       | >= 375.26 |  >= 376.51 |
| CUDA 8.0 (8.0.44)           | >= 367.48 |  >= 369.30 |
| CUDA 7.5 (7.5.16)           | >= 352.31 |  >= 353.66 |
| CUDA 7.0 (7.0.28)           | >= 346.46 |  >= 347.62 |

很不幸，我的系统上自动安装的显卡驱动版本为 390 系列，但是我所要安装的 CUDA 版本为 10.0.130，所以必须将显卡驱动升级。这便是暴风雨的开端。

#### 升级 NVIDIA 显卡驱动版本

首先，我们提出一个退而求其次的方法，如果你不想重新安装显卡驱动，那么就不能安装最新版本的 CUDA，对于我来说，系统自动安装的显卡驱动只能支持到 CUDA 9.0，那么我就需要去下载 CUDA 9.0 的安装包然后安装 CUDA 9.0，CUDA 的版本本质上讲没有什么太大的区别，只不过里面有许多更新，由于我先安装的 CUDA，所以我不想将 CUDA 再次卸载掉，所以选择升级显卡驱动版本。

如果手动升级 NVIDIA 显卡驱动版本至最新的型号，需要根据显卡的型号到 NVIDIA 官网查询，使用如下命令查询 NVIDIA 显卡的型号：

```shell
nvidia-smi -L
```

然后，去 NVIDIA 官网下载最新的驱动，[点击这里查询][NVIDIA-DRIVER-VERSION]。

[NVIDIA-DRIVER-VERSION]: https://www.nvidia.cn/Download/index.aspx?lang=cn "NVIDIA-DRIVER-VERSION"

查询到显卡驱动后，点击下载 Linux 系统版本的显卡驱动，下载之后是一个扩展名为 .run 的文件，接下来我们就需要通过 .run 文件的方式安装 NVIDIA 的显卡驱动。

通过 .run 文件的方式安装显卡驱动并不是特别容易，需要我们关闭 Ubuntu 的图形界面，然后在命令行模式下进行安装，安装好之后再开启图形界面，其中还包含一些其他方面的问题，比如防止循环登录等（这些问题虽然我没有遇到，但是网上的教程都是这么说的，所以我就只好这样做了）。

Ubuntu 18.04 版本做了比较大的改动，很多命令与之前的版本都不同了，比如关闭和开启图形界面，以及从图形界面切换进入命令行和从命令行切换进入图形界面。这里 Ubuntu 18.04 从图形界面切换进入命令行模式的快捷键为：`Ctrl+Alt+F4`，从命令行切换回当前的图形界面的快捷键为`Ctrl+Alt+F2`。

关闭图形界面的命令为：

```shell
sudo systemctl set-default multi-user.target
sudo reboot
```

开启图形界面的命令为：

```shell
sudo systemctl set-default graphical.target
sudo reboot
```

以上两个命令中都要重启后生效，即重启后默认会直接进入命令行或者直接进入图形界面。

接下来我们进入安装步骤：

1. **禁用 nouveau**

    我并不清楚这个 nouveau 是干什么用的，但是网上的教程说 Ubuntu 16.04 默认安装了第三方开源的驱动程序 nouveau，安装 NVIDIA 显卡驱动首先要禁用 nouveau，不然会碰到冲突的问题，导致无法安装显卡驱动。虽然不清楚 Ubuntu 18.04 是否也有同样的问题，但最好还是执行。

    禁用步骤如下：

    编辑黑名单

    ```shell
    sudo gedit /etc/modprobe.d/blacklist.conf
    ```

    然后在该文件最后添加如下几行：

    ```
    blacklist nouveau
    options nouveau modeset=0
    ```

    之后保存文件，然后在命令行执行如下命令：

    ```shell
    sudo update-initramfs -u
    ```

    然后执行：

    ```shell
    sudo reboot
    ```

    重启系统。

    之后验证 nouveau 是否已经禁用，执行

    ```shell
    lsmod | grep nouveau
    ```

    如果没有信息显示，说明 nouveau 已经被禁用（对这一点我持怀疑态度，因为在 Ubuntu 18.04 系统上，不需要执行前面的添加黑名单步骤，直接在命令行中运行 `lsmod | grep nouveau` 的话也是没有任何信息显示的，所以第一遍安装驱动时，我就没有执行添加黑名单的步骤，但之后在安装驱动时第一遍没有安装成功，原因可能不是由此而起，但是也不确定，所以鉴于此，为了保险起见，还是执行禁用 nouveau 的这个步骤）。


2. **下载驱动**

    按照之前[我介绍的网址][NVIDIA-DRIVER-VERSION]，下载相应的驱动，这里我下载的驱动文件名为：`NVIDIA-Linux-x86_64-410.93.run`，即版本号为 410.93 的显卡驱动。然后我们将其复制到 `\home` 目录下（因为在一会儿命令行模式下进行操作，命令行模式下中文不能正常显示，所以不要将其放在一个中文名在内的目录下，最好直接将其放在用户家目录下）。

3. **安装驱动**

    现在我们在图形界面的终端下执行命令：

    ```shell
    sudo systemctl set-default multi-user.target
    ```

    用来关闭图形界面（放心，不是立即就会关闭图形界面，而是再你下次开机时直接进入命令行模式，不会进入图形界面）。

    然后执行：

    ```shell
    sudo reboot
    ```

    以重启系统，这样，下次你在进入 Ubuntu 时就会直接进入命令行模式。

    现在我们重启完毕，已经处在命令行模式下了（注意，命令行模式下会首先登录，此时输入用户名和密码，注意一定不能使用小键盘，因为此时小键盘不起作用），执行如下命令：

    ```shell
    sudo apt-get remove --purge nvidia-*
    ```

    用来彻底卸载已安装的显卡驱动程序。

    然后由于我们之前已经将 .run 驱动安装文件复制到 `/home` 目录下，所以我们在当前用户模式下输入 `ls`，会查看到驱动文件。之后执行：

    ```shell
    sudo chmod 755 NVIDIA-Linux-x86_64-版本号.run
    ```

    用于获取权限。

    之后安装驱动，执行：

    ```shell
    sudo ./NVIDIA-Linux-x86_64-版本号.run -no-x-check -no-nouveau-check -no-opengl-files
    ```

    其中有几个参数说明如下：

    ```
    -no-x-check : 表示安装驱动时关闭 X 服务
    -no-nouveau-check : 表示安装驱动时禁用 nouveau
    -no-opengl-files : 表示只安装驱动文件，不安装 opengl 文件，可以避免出现循环登录的问题
    ```

    以上参数的具体含义其实我并不清楚，但是网上的教程确实提及了这几个问题，并且我在安装过程中也执行了这些命令，并最终安装成功，所以还是加上这些参数，以防万一。

    在安装过程中会有一些选项，有几个需要注意，当问到是否 `register the kernel module sources with DKMS`，要选择 `no`；当问到是否安装 `32-bit compatibility libraries`，可以选择 `yes`，也可以选择 `no`；当问到是否运行 `x-config` 时，一定要选择 `yes`。

    安装完成后，运行

    ```shell
    sudo update-initramfs -u
    ```

    之后，运行

    ```shell
    sudo reboot
    ```

    重启系统，将再次进入命令行模式，登陆后，我们执行：

    ```shell
    nvidia-smi
    ```

    如果出现如下信息：

    ```
    Mon Jan 21 17:16:13 2019
    +-----------------------------------------------------------------------------+
    | NVIDIA-SMI 410.93       Driver Version: 410.93       CUDA Version: 10.0     |
    |-------------------------------+----------------------+----------------------+
    | GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
    | Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
    |===============================+======================+======================|
    |   0  GeForce GT 630      Off  | 00000000:01:00.0 N/A |                  N/A |
    | 25%   39C    P8    N/A /  N/A |    133MiB /   978MiB |     N/A      Default |
    +-------------------------------+----------------------+----------------------+

    +-----------------------------------------------------------------------------+
    | Processes:                                                       GPU Memory |
    |  GPU       PID   Type   Process name                             Usage      |
    |=============================================================================|
    |    0                    Not Supported                                       |
    +-----------------------------------------------------------------------------+
    ```

    则代表安装成功，这时，我们执行如下命令：

    ```shell
    sudo systemctl set-default graphical.target
    ```

    重新启动图形界面，此时，再次重启系统，系统就会进入图形界面，如果足够幸运的话，你在图形界面终端下再次执行

    ```shell
    nvidia-smi
    ```

    会查询到和上面相同的信息，然后再执行

    ```shell
    cat /proc/driver/nvidia/version
    ```

    会查询到你刚才安装的显卡驱动的信息，这基本上就完全成功了。

    但是，有可能事实不是这样顺利，你可能会再次进入图形界面是面临显示器分辨率失真，并且 gnome control center 崩溃的事实，然后显卡驱动信息查询不到（就是我），这代表你的驱动安装失败了（我也很纳闷，明明在命令行模式下显示安装成功了，在回到图形界面后又失败了），没有办法，你还要重新安装。关于安装过程中出现的错误，我出现了一个大家都比较常见的错误，描述如下。

4. **安装过程中的错误**

    我在第一遍安装后，进入图形界面后面临显示器分辨率失真的问题，并且刚刚在命令行模式下安装好的显卡驱动查询不到了，没有办法，我又开始了第二遍安装，在第二遍安装时，遇到了如下错误：

    ```
    ERROR: Unable to load the kernel module 'nvidia.ko'.
    This happens most frequently when this kernel module was built against
    the wrong or improperly configured kernel sources, with a version of
    gcc that differs from the one used to build the target kernel, or if
    a driver such as rivafb/nvidiafb is present and prevents the NVIDIA
    kernel module from obtaining ownership of the NVIDIA graphics device(s),
    or NVIDIA GPU installed in this system is not supported by this NVIDIA
    Linux graphics driver release.
    ```

    在网上找到了解决方法，如下：

    重新回到图形界面下，添加黑名单，执行

    ```shell
    sudo gedit /etc/modprobe.d/blacklist.conf
    ```

    在文件末尾添加如下行：

    ```
    blacklist vga16fb
    blacklist nouveau
    blacklist rivafb
    blacklist nvidiafb
    blacklist rivatv
    ```

    保存并退出，再次彻底完全删除已安装的 NVIDIA 显卡驱动，执行：

    ```shell
    sudo apt-get --purge remove nvidia-*
    ```

    之后再进入命令行模式，执行上述安装步骤，应该不会再出现类似错误。

## 安装 CUDA 10.0

以上将 NVIDIA 显卡驱动程序安装好之后，再安装 CUDA。或者你也可以首先安装 CUDA，然后再安装 NVIDIA 显卡驱动程序（例如我）。

首先去 [NVIDIA 官网](https://developer.nvidia.com/cuda-downloads)下载 CUDA，注意选对应操作系统的版本，这里我选择下载的是 Ubuntu 18.04 下的 .run 文件。然后查看我们自己系统上的 gcc 和 g++ 编译器的版本号，18.04 一般预装 gcc 与 g++ 版本号为 7.3，CUDA 10.0 是支持这个版本的 gcc 与 g++ 的，所以我们完全可以直接安装 10.0.130 版本的 CUDA。

下载完后的 .run 文件名为：`cuda_10.0.130_410.48_linux.run`，我们按照官网上的说明执行如下命令：

```shell
sudo sh cuda_10.0.130_410.48_linux.run
```

进行安装，在安装过程中会有几个问题，其中有一个提示是否安装显卡驱动，加入我们电脑上之前自己没有安装过显卡驱动的话，可以选择 `yes`，但是如果选 `yes` 的话，这个显卡驱动不一定能安装成功。由于我们选择自己手动安装显卡驱动，所以这里我们选择 `no`，然后后面的选项全部都安装，最后还要配置环境变量：

```shell
sudo gedit ~/.bashrc
```

如果你用的命令行不是 bash 而是 zsh 的话，同样使用如下命令为 zsh 添加环境变量：

```shell
sudo gedit ~/.zshrc
```

这时在打开的文件末尾加上如下的两行：

```
export PATH=/usr/local/cuda-10.0/bin${PATH:+:${PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda-10.0/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}} 
```

注意，先查看自己的 CUDA 版本号再添加环境变量。

至此，CUDA 安装成功，然后这时候你就可以 Make 自己的 CUDA Samples，然后查看运行效果了。
