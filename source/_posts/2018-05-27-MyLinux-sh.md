---
title: MyLinux.sh
tags: Linux应用
abbrlink: 6c3d6e2f
date: 2018-05-22 09:57:06
---


```
#/bin/bash

# 使用Elementary OS 0.4

#个别系统不能增加ppa
sudo apt install software-properties-common

sudo apt install vim

if [ "$?" -ne 0 ]
	then
		exit 1
fi

sudo apt install vifm

if [ "$?" -ne 0 ]
	then
		exit 2
fi

#添加中文环境 Settings >> Language & Region >> unLock >> Complete Installation >> click on English in the left sidebar >> Set System Language
#安装fcitx五笔，其他相关包也会安装，fcitx\fcitx-config-gtk\fcitx-config-common
sudo apt install fcitx-table-wubi

#baka-player
#http://bakamplayer.u8sand.net/installation.php
cd ~/Download
sudo apt install wget
wget https://github.com/u8sand/Baka-MPlayer/releases/download/v2.0.4/baka-mplayer_2.0.4-1_amd64.deb
sudo dpkg -i baka-mplayer_2.0.4-1_amd64.deb

#filezilla
sudo apt install filezilla

#virtual studio code
sudo apt install code

#virtualbox
sudo apt install virtualbox-5.2

```