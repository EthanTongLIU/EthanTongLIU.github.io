---
layout:       post
title:        "Wamp Server 部署若干问题"
subtitle:     "若干Wamp Server网站部署问题与排故记录"
date:         2020-12-02 23:24:00
author:       "LiuTong"
header-img:   "img/post-bg-website.jpg"
header-mask:  0.3
catalog:      true
multilingual: false
tags:
    - 服务器
    - 网站搭建
---

总结一下在Wamp server服务器部署中遇到过的种种问题，供以后参考。

### Wamp Server安装中或安装后启动时，提示丢失msvcp.dll和msvcr.dll这些动态链接库
   
是由于电脑中缺乏微软的一些运行库造成的，遇到这种情况要去下载微软常用运行库，不要单独安装msvcp.dll或者msvcr.dll，因为这样可能还是不全。

### 网站部署好之后，只能在本地服务器访问，局域网内无法访问

是由于Apache的配置文件没有修改，Apache的配置文件默认将外网的访问权限设为了禁止，因此需要修改Apache的配置文件。

- 第一个文件httpd.conf，第289行附近：

    原文为：
    ```
    #
    # Controls who can get stuff from this server.
    #
    # onlineoffline tag - don't remove
      Require all denied
    ```
    修改为：
    ```
    #
    # Controls who can get stuff from this server.
    #
    # onlineoffline tag - don't remove
      Require all granted
    ```

- 第二个文件httpd-vhosts.conf

    原文为：
    ```
    # Virtual Hosts
    #
    <VirtualHost *:80>
      ServerName localhost
      ServerAlias localhost
      DocumentRoot "${INSTALL_DIR}/www"
      <Directory "${INSTALL_DIR}/www/">
        Options +Indexes +Includes +FollowSymLinks +MultiViews
        AllowOverride All
        Require all denied
      </Directory>
    </VirtualHost>
    ```
    修改为：
    ```
    # Virtual Hosts
    #
    <VirtualHost *:80>
      ServerName localhost
      ServerAlias localhost
      DocumentRoot "${INSTALL_DIR}/www"
      <Directory "${INSTALL_DIR}/www/">
        Options +Indexes +Includes +FollowSymLinks +MultiViews
        AllowOverride All
        Require all granted
      </Directory>
    </VirtualHost>
    ```

简单来说就是将这两处的denied修改为granted，修改完之后，再关闭本服务器的所有防火墙，之后就可以在局域网内其他电脑上访问了。

