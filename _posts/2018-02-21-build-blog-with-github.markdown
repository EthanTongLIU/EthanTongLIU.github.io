---
layout:       post
title:        "基于 Github 搭建个人博客全过程详解"
subtitle:     "基于 Github Pages 功能，应用 Jekyll 模板，搭建使用 Markdown 写作的个人博客"
date:         2018-02-21 19:13:00
author:       "LiuTong"
header-img:   "img/in-post/post-bg-github-blog.jpg"
header-mask:  0.3
catalog:      true
multilingual: true
tags:
    - Github
    - Jekyll
    - Markdown
---

> 路漫漫其修远兮，吾将上下而求索。
>               ------ 屈原 《离骚》


本文将详细介绍我的个人博客的搭建全过程，争取详细详细再详细，有搭建个人博客愿景的朋友可以参考本文所介绍的方法。在开始之前，我先给出几个网址：

* [OPEN文档 - Github + Markdown + Jekyll 打造完美的个人博客][link-1]
* [CSDN - 如何快速搭建自己的github.io博客][link-2]
* [简书 - 在 Github 上写博客][link-3]
* [知乎 - 有哪些简洁明快的 Jekyll 模板？][link-4]

  [link-1]: http://www.open-open.com/doc/view/1556d9148651413cba791ee0edb347e9

  [link-2]: http://blog.csdn.net/walkerhau/article/details/77394659?utm_source=debugrun&utm_medium=referral

  [link-3]: https://www.jianshu.com/p/1260517bbedb

  [link-4]: https://www.zhihu.com/question/20223939

因为我的工作是参考网上这些教程所集成而来的，仍然是那句话:**我站在了巨人的肩膀上**。然而前人们的教程我参考了好多，不过可能还是有一些细节的地方讲的不是很清楚，比如我这个小白在建站的时候就遇到了很多的困难，下面我尽量通过详细的讲解以求用让大家用最快的速度搭建好个人博客。

## OVERVIEW：原料和工序

先来说一下整个过程需要用到的工具：

*  Git：是一个开源的分布式版本管理系统，好吧，我承认我也不知道这是啥意思。如果说点人话的话，它就像一个云盘一样（可以联想一下百度网盘），专门用来存储个人项目，主要是代码之类，全球程序员都在用，它不仅可以方便地上传和下载你的代码，使本地文件和云端保持同步，还可以详细的记录你每一次对文件所做的修改，可以执行版本回退。

* Github：Github 就是上面所说的云端，是全球所有支持 Git 托管服务平台中最大，最流行的一个，国内目前类似的平台有码云。全球程序员都在用 Github，它就像一个大服务器一样，你通过 Git 将本地主机上的项目上传到 Github，让 Github 来帮你托管。在这上面你可以与他人分享千千万万的优秀的开源项目。本文中所介绍的 Blog 搭建过程就是利用 Github 的 Pages 功能，本质上也是在本地建一个仓库，里面放入你的博客的所有东西（网页源码，各种配置文件源码等等），然后上传到 Github 上，Github 作为你的 Blog 的服务器，并且给你一个免费的域名，将你的 Blog 发布出去，其他人就可以通过搜索你的域名访问你的 Blog 网站了。

* Jekyll：Jekyll 的核心其实是一个文本转换引擎。它的概念其实就是： 你用你最喜欢的标记语言来写文章，可以是 Markdown，也可以是 Textile，或者就是简单的 HTML，然后 Jekyll 就会帮你套入一个或一系列的布局中。在整个过程中你可以设置URL路径，你的文本在布局中的显示样式等等。这些都可以通过纯文本编辑来实现，最终生成的静态页面就是你的成品了。

* Markdown：轻量级标记语言，语法简洁，易学易懂，适合写博客，本文中搭建的博客平台就需要使用 Markdown 写作，博客的源文件就是 Markdown 文件。

* DNS 提供商：也就是你买域名的地方，比方说我这个域名*iliutong.cn*是在万网注册的，万网就是我的 DNS 提供商。当然完全可以不用买这个个性域名，个性域名不是永久的而且还要花钱，Github 提供的域名是免费的而且还永久（只要你不删除这个博客项目），但是一个博客要体现出自己的个性、好记而且还希望给人耳目一新的感觉，最好还是去注册一个个性域名。

接着大致介绍一下整个过程的步骤：

* 注册一个 Github 的账号

* 在本地安装 Git

* 为你的博客项目建一个 Repo（仓库），开启 Pages 功能，然后将这个 Repo 复制到本地

* 搞一个博客项目的模板，该模板要符合 Jekyll 要求的格式，然后将模板复制到你的本地 Repo 中，稍作修改

* 将修改好的所有内容上传到 Github 中，这时就已经可以通过 Github 指定的域名访问个人博客了

* 去万网购买一个域名

* 通过特定的文件将 Github 服务器和你的域名联系起来，通常称之为域名解析

* 搭建过程结束，这时就可以通过你的域名访问个人博客了

* * *

好了，接下来开始介绍每一个步骤的具体操作。

* * *

## 使用 Github 建立本地仓库

### 注册 Github 的账号

如果你还不是 Github 的成员，请前往 [Github](https://github.com/) 注册一个自己的账号，类似于各种网站的注册过程，非常简单。

### 在本地安装 Git

注意一点，Git 和 Github 不是一个东西，你需要使用 Git 来管理项目而 Github 只是托管你的项目代码的云服务器。我的搭建环境是 Windows，所以在本地安装 Git 方便，前往[这里](https://git-scm.com/download/win)下载你需要的 Windows 安装程序，然后双击 .exe 文件直接安装即可，注意记住你的安装目录。如果安装成功，此时在桌面上右击鼠标，就会看到两个图标，分别是`Git GUI Here`和`Git Bash Here`，在接下来的所有过程中，我们一直使用`Git Bash Here`。

如果你希望直接在 Windows 自带的命令行下运行 Git 的命令（即不使用 Git 自带的命令行 Git Bash，而直接打开 Windows 自带的命令行运行 Git 命令），这时候就需要把 Git 加到 Windows 的环境变量中去，方法就是找到 Git 安装目录下的 bin 文件夹，将其加到 Windows 的环境变量中去，Win10 如何添加环境变量请参考[这里](https://jingyan.baidu.com/article/47a29f24610740c0142399ea.html)。

### 为你的博客项目建一个 Repo

所谓 Repo 就是仓库的意思，这里我们要建立的博客其实就是一个项目，我们可以通过像管理普通项目一样管理我们的博客。

* *登录自己的 Github，点击 + 号，创建新仓库*

  ![](img/in-post/post-github-new-repo.jpg)

* *初始化新仓库，注意个人博客项目仓库的文件名必须写成 username.github.io 的格式，这里的 username 是你的账号名，比如我的名字是 EthanTongLIU，我的仓库的名字就取为 EthanTongLIU.github.io*

  ![](img/in-post/post-github-init-new-repo.jpg)

* *已经创建好的新仓库*

  ![](img/in-post/post-github-new-repo-done.jpg)