---
layout:     post
title:      渗透测试系统parrot与个性化conky搭建
subtitle:   
date:       2017-07-13
author:     sp4rk
header-img: 
catalog: true
tags:
    - Linux	
    - pentest
---

#### 0x00

最近想装一个linux的系统，那么为什么选择鹦鹉呢，因为鹦鹉作为渗透测试系统，工具是远远多余Kali的，并且拥有众多编辑器和办公软件。

镜像： [64位系统种子文件](https://mirrordirector.archive.parrotsec.org/parrot/iso/3.7/Parrot-full-3.7_amd64.iso.torrent)

成果图附上
![](/img/e.jpg)


<!--more-->


parrot中文社区：[parrot中文社区](https://parrotsec-china.org/)
#### 0x01
初始配置（常用的软件QQ，网易云等下载）可以参考：[初始设置](https://parrotsec-china.org/t/parrot/36)，需要注意的是，我这里下载的是wineqq,用crossover下载会出现乱码问题。　

接下来说一下桌面美化，由于该系统是mate的桌面，所以我用的是cairo-dock,Cairo-dock（桌面下方的dock栏）下载：[cairo-dock_3.4.1-1_i386.deb](http://ftp.br.debian.org/debian/pool/main/c/cairo-dock/cairo-dock_3.4.1-1_i386.deb)
cairo-dock deb包下载
```php
sudo dpkg -i cairo-dock_3.4.1-1_i386.deb
```
设置开机启动：左下角右键->cairo-dock->开机启动，上面的面板设置，多余的图标右键删除，（这里不要把通知区域删除了，不然到时候没有wifi图标），误删了网上的解决方法是编辑NetworkManage配置文件。
```php
sudo gvim /etc/NetworkManager/NetworkManager.conf
```
 将false改为true(我这里没成功)，右键上面添加新的panel(找到Notification Area)即可。删除桌面上多余的图标
```php
打开Applications Menu->Preferences->Look and Feel->Mate Tweak->Desktop
```
去掉desktop icons下面选项即可。
conky(硬件资源实时监控)安装可以参照：[关于Parrot-Conky的安装](https://parrotsec-china.org/t/parrot-conky/157)
[Conky搭建属于Parrot的硬件资源实时监控](https://parrotsec-china.org/t/conky-parrot/128)