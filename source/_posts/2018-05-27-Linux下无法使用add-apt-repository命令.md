---
title: Linux下无法使用add-apt-repository命令
tags: Linux应用
abbrlink: 5ff43231
date: 2018-05-01 15:59:11
---

Linux下无法使用add-apt-repository命令报错：
`add-apt-repository command not found`

解决办法：
```
sudo apt install python-software-properties software-properties-common 
sudo apt update
```