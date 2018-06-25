---
title: 【Linux】Linux权限管理04 sudo权限
tags: Linux
abbrlink: 6ca02215
date: 2018-04-27 15:37:05
---

## 目的
赋予普通用户超级管理员的权限

## 使用
```
visudo
#实际修改的是/etc/sudoers文件
#sudo的配置文件是/etc/sudoers。visudo会锁住sudoers文件，保存修改到临时文件/etc/sudoers.tmp，然后检查文件格式，确保正确后才会覆盖sudoers文件。必须保证sudoers格式正确，否则sudo将无法运行。

root    ALL=(ALL)                       ALL
用户名   被管理主机的地址=（可使用的身份）    授权命令（绝对路径）
%wheel  ALL=(ALL)                       ALL
%组名    被管理主机的地址=（可使用的身份）    授权命令（绝对路径）

第二个all指，可以切换成任意身份，这个可以直接省略

$sudo -l 
#查看可以执行的命令

$sudo /sbin/shutdown -r now
#普通用户执行超级命令的时候必须要加 sudo 命令的绝对路径
```


补充：
* http://www.cnblogs.com/Jimmy1988/p/7270881.html
*  
https://wiki.archlinux.org/index.php/Sudo_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)#.E6.9F.A5.E7.9C.8B.E5.BD.93.E5.89.8D.E8.AE.BE.E7.BD.AE
http://man.linuxde.net/sudo?zwbizq=f0evo1
