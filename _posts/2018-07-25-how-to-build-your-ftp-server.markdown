---
layout:       post
title:        "如何搭建一个局域网 FTP 服务器"
subtitle:     "使用 win10 自带 IIS 服务搭建局域网 FTP 服务器并将其与你的 Web 服务器关联"
date:         2018-07-25 23:25:00
author:       "LiuTong"
header-img:   "img/in-post/post-bg-how-to-put-online-your-wampserver.jpg"
header-mask:  0.3
catalog:      true
tags:
    - 服务器搭建
    - 计算机网络
---

最近在学习 PHP，由于 PHP 脚本必须运行在服务器端，所以必须要搭建一个 Web 服务器，于是我就使用 Wamp（Apache + MySQL + PHP）搭建了一个局域网 Web 服务器，但是这样搭建的 Web 服务器在上传文件时必须在服务器所在主机上上传文件，并且文件的上传方式为复制粘贴。如果可以搭建一台局域网 FTP 服务器，让 FTP 服务器和 Web 服务器的主目录相同，这样就可以实现使用 FTP 协议来进行局域网内文件上传，并且可以实现在局域网内其他主机向 FTP 服务器上传文件，实现多人管理的目的。

## 使用 win10 自带 IIS 服务搭建局域网 FTP 服务器

搭建局域网 FTP 服务器的方式很多，这里其实借助 win10 自带的 IIS 功能就可以搭建一台 FTP 服务器，搭建过程并不复杂，过程如下：

* **打开控制面板**

![img](/img/in-post/post-build-ftp-server-1.PNG)

![img](/img/in-post/post-github-new-repo.JPG)

<img src="../img/in-post/post-build-ftp-server-1.PNG" alt="1.PNG" width="800">

<img src="../img/in-post/post-build-ftp-server-2.PNG" alt="2.PNG" width="600">

<img src="../img/in-post/post-build-ftp-server-3.PNG" alt="3.PNG" width="400">

<img src="../img/in-post/post-build-ftp-server-4.PNG" alt="4.PNG" width="400">

<img src="../img/in-post/post-build-ftp-server-5.PNG" alt="5.PNG" width="400">

<img src="../img/in-post/post-build-ftp-server-6.PNG" alt="6.PNG" width="800">

<img src="../img/in-post/post-build-ftp-server-7.PNG" alt="7.PNG" width="800">

<img src="../img/in-post/post-build-ftp-server-8.PNG" alt="8.PNG" width="800">

<img src="../img/in-post/post-build-ftp-server-9.PNG" alt="9.PNG" width="800">

<img src="../img/in-post/post-build-ftp-server-10.PNG" alt="10.PNG" width="800">

<img src="../img/in-post/post-build-ftp-server-11.PNG" alt="11.PNG" width="800">

<img src="../img/in-post/post-build-ftp-server-12.PNG" alt="11.PNG" width="800">

<img src="../img/in-post/post-build-ftp-server-13.PNG" alt="11.PNG" width="800">

<img src="../img/in-post/post-build-ftp-server-14.PNG" alt="11.PNG" width="800">

<img src="../img/in-post/post-build-ftp-server-15.PNG" alt="11.PNG" width="800">

<img src="../img/in-post/post-build-ftp-server-16.PNG" alt="11.PNG" width="800">

## 为 FTP 服务器设置用户权限

## 将 FTP 服务器根目录设置为 Web 服务器根目录

## 使用 FTP 上传工具上传和下载文件

## 问题处理：解决 Web 服务器和 FTP 服务器端口冲突问题

#### 问题描述

#### 问题解决