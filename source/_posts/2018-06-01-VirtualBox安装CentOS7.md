---
title: VirtualBox安装CentOS7
tags: Linux
abbrlink: '85425272'
date: 2018-06-01 20:51:25
---


主要的安装过程不再详细说明，主要是针对安装后进行的一些配置进行说明

1. 网络设置为桥接，因为本地要和虚拟机进行通信
2. centos7默认没有安装ifconfig命令，而一开始也是上不了网的，可以使用`ip addr`命令代替，也可以通过`yum provides ifconfig`查找对应的安装包，可以知道是net-tools，安装net-tools即可，当然在无法连网时，使用ip addr是为了获得网卡名称
3. `vi /etc/sysconfig/network-scripts/ifcfg-eth0`，修改对应文件名的配置文件，改`onBoot`为yes，重启网络`systemctl restart network`，这时可以ping或安装ifconfig

<!--more-->
