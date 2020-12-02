---
layout:       post
title:        "将 Git 连接方式修改为 SSH"
subtitle:     "将 Git 远程连接方式从https修改为SSH，并且在SSH失效后的设置办法"
date:         2020-12-02 23:24:00
author:       "LiuTong"
header-img:   "img/in-post/post-bg-github-blog.jpg"
header-mask:  0.3
catalog:      false
multilingual: false
tags:
    - Git
---

Git的远程链接方式如果为https，则每次上传到远程主机过程中都要输入账号和密码，很不方便，所以需要将本地和远程的连接方式从https修改为ssh，如果原来设置过ssh，一段时间后不知道因为什么原因失效了，可以参考以下的解决方法：

- 首先：进入`C:\Users\你的用户名\.ssh`目录下，一般会发现三个文件：

  id_rsa
  id_rsa.pub
  known_hosts

  因为已经失效了，所以将这三个文件全部删除。

- 之后打开git bash

  运行命令`ssh-keygen -t rsa -C xxxxxx@xxx.com(此处填自己邮箱地址)`    

  之后会遇到一大推提示命令我们全部直接回车即可。完毕在原目录下会发现新建的两个文件: id_rsa和id_rsa.pub。复制id_rsa.pub里面的内容到github.com你的远程仓库上。

- 此时我们再打开git 控制台，在拉取代码或者提交代码前会弹出重新连接的提示命令，我们选择yes就行了。



