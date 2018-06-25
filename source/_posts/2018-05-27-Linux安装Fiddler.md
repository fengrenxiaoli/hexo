---
title: Linux安装Fiddler
tags: Linux应用
abbrlink: dd92ed66
date: 2018-05-11 10:01:44
---


1. 安装mono：
```
sudo apt install mono-complete
```
2. 下载Fiddler
https://www.telerik.com/download/fiddler

3. 用mono安装它，从提取的目录中运行：
```
mono Fiddler.exe

#或者运行
mono /path/to/extracted/fiddler/Fiddler.exe
```
