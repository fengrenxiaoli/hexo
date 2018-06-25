---
title: Ubuntu 安装、卸载wine
tags: Linux应用
abbrlink: 8d5353b1
date: 2018-05-01 16:17:58
---





### 安装
```
sudo add-apt-repository ppa:wine/wine-builds
sudo apt-get update
sudo apt-get install --install-recommends wine-staging
sudo apt-get install winehq-staging
```
<br />

### 卸载
```
sudo apt purge wine
rm -r ~/.wine
sudo apt-get autoremove
#清理wine模拟运行的windows程序:
sudo rm -r /home/username/.local/share/applications
#清理残余的windows程序:
sudo rm -r /home/username/.config/menus/applications-merged/wine*
```
* apt-get remove 会删除软件包而保留软件的配置文件
* apt-get purge 会同时清除软件包和软件的配置文件


