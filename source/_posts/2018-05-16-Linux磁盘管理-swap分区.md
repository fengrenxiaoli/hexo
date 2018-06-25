---
title: 【Linux】Linux磁盘管理06 swap分区
tags: Linux
abbrlink: 92783d86
date: 2018-05-06 15:59:06
---


为磁盘添加SWAP交换分区

1.首先建立一个普通的Linux分区
```
fdisk /dev/sdb
#参考MBR分区
#输入p查看sdb的分区
```

2.修改分区类型的16进制编码
```
输入t，输入要修改的磁盘编号 这里是 6（sdb6的6）；
输入 L 来查看可以修改成的类型
再输入82（Linux swap）,保存成功
输入p来查看已经保存的情况
输入w保存分区
```
![](http://ow3dy62zt.bkt.clouddn.com/IMG30.jpg)


3.格式化交换分区
```
mkswap（后面跟随设备名称） /dev/sdb6 完成格式化
```

4.启动交换分区
```
swapon /dev/sdb6 	#启动交换分区
free 				#查看加载状况
swapoff /dev/sdb6 	#关闭交换分区
```
