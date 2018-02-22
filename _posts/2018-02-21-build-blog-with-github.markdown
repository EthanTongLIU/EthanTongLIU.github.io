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

* Git：是一个开源的分布式版本管理系统，好吧，我承认我也不知道这是啥意思。如果说点人话的话，它就像一个云盘一样（可以联想一下百度网盘），专门用来存储个人项目，主要是代码之类，全球程序员都在用，它不仅可以方便地上传和下载你的代码，使本地文件和云端保持同步，还可以详细的记录你每一次对文件所做的修改，可以执行版本回退。

* Github：Github 就是上面所说的云端，是全球所有支持 Git 托管服务平台中最大，最流行的一个，国内目前类似的平台有码云。全球程序员都在用 Github，它就像一个大服务器一样，你通过 Git 将本地主机上的项目上传到 Github，让 Github 来帮你托管。在这上面你可以与他人分享千千万万的优秀的开源项目。本文中所介绍的 Blog 搭建过程就是利用 Github 的 Pages 功能，本质上也是在本地建一个仓库，里面放入你的博客的所有东西（网页源码，各种配置文件源码等等），然后上传到 Github 上，Github 作为你的 Blog 的服务器，并且给你一个免费的域名，将你的 Blog 发布出去，其他人就可以通过搜索你的域名访问你的 Blog 网站了。

* Jekyll：我也不太了解，但是搭建过程里面用到了，应该是一个渲染网页的东西，Github 支持，在操作过程中不太多涉及该工具。

* Markdown：轻量级标记语言，语法简洁，易学易懂，适合写博客，本文中搭建的博客平台就需要使用 Markdown 写作，博客的源文件就是 Markdown 文件。

* DNS 提供商：也就是你买域名的地方，比方说我这个域名*iliutong.cn*是在万网注册的，万网就是我的 DNS 提供商。当然完全可以不用买这个个性域名，个性域名不是永久的而且还要花钱，Github 提供的域名是免费的而且还永久（只要你不删除这个博客项目），但是一个博客要体现出自己的个性、好记而且还希望给人耳目一新的感觉，最好还是去注册一个个性域名。

## 使用 Github 建立本地仓库

## 找一个 Jekyll 的 Blog 模板

## 学会使用 Markdown 写作

## 给 Blog 配置自己的个性域名

### 域名注册

### 域名解析