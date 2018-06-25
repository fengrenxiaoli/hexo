---
title: Linux安装Cisco Packet Tracer
tags: 计算机网络
abbrlink: 5aea5c58
date: 2018-06-22 10:16:28
---


## 下载
https://www.netacad.com/courses/packet-tracer

注册下载

<!-- ## 环境
需要java和32位包

```
sudo dpkg --add-architecture i386
sudo apt-get install libc6:i386
sudo apt-get install lib32z1 lib32ncurses5 lib32bz2-1.0
sudo apt-get install libnss3-1d:i386 libqt4-qt3support:i386 libssl1.0.0:i386 libqtwebkit4:i386 libqt4-scripttools:i386
``` -->

## 安装

将压缩文件解压缩到一个文件夹并打开一个终端。
使用sudo权限运行install.sh并按照说明进行安装
安装的默认路径是`/opt/pt`


## 运行


在终端中键入`packettracer`来运行
如果没有反应，运行`/opt/pt/bin/PacketTracer7`
会提示缺少库文件，安装相应库文件后再次运行



## 配置

**调整终端字体**
点击选项（options），点击首选项（Preferences），然后点击字体（Font），选CLI右侧就有大小的选择了。





参考：
- http://www.christospanoudis.com/how-to-install-packet-tracer-7-1-in-linux-and-resolve-any-dependency-issues/
- https://linux.cn/article-5576-1.html







