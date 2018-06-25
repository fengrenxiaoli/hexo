---
title: VirtualBox扩容
tags: Linux应用
abbrlink: 93ab716c
date: 2018-05-14 10:22:32
---

这篇文章用于在VirtualBox虚拟机上的CentOS扩充根目录空间，区别于新增硬盘以及添加新的挂载点

主机环境为Ubuntu 17.04
VirtualBox 5.2
虚拟机为CentOS 7

VirtualBox 中虚拟硬盘有几种形式，VMDK、VDI、VHD、HDD等。
* VMDK：是VMware开发并使用的，同时也被SUN的xVM、QEMU、SUSE Studio、.NET DiscUtils支持，所以兼容性会好些。
* VDI：是Virtual Box 自己的处理格式，而且Virtual Box支持Windows和Linux，所以对于使用VirtualBox的用户比较好。
* VHD：是Windows专有的处理格式，HDD是Apple专有的处理格式，所以不会支持跨平台，一般不会考虑。




## 扩容磁盘文件
在主机上操作
VBoxManage命令是安装VirtualBox时安装的，如果无法使用该命令，请指定完整路径，寻找VirtualBox的安装目录，我的在`/usr/bin`下
centos.vdi和centos.vmdk是VirtualBox创建的系统磁盘文件，一般位于用户的`VirtualBox VMs`文件夹下

```
#VDI
#单位为M
VBoxManage modifyhd centos.vdi --resize 16000

#VMDK
#单位为M
#如果是VMDK就要先转换成VDI，然后再扩容
#vmdk是转换前的文件，vdi是转换之后的文件
VBoxManage clonehd "centos.vmdk" "centos.vdi" --format vdi
VBoxManage modifyhd "centos.vdi" --resize 16000
#如果想再转回为VMDK，用这个命令就可以了，直接使用vdi格式也可以
VBoxManage clonehd "centos.vdi" "resized.vmdk" --format vmdk
```


## 指定新磁盘文件
打开虚拟机，选择系统 > 右击 > 设置 > 存储 > 控制器SATA > 右边的添加虚拟硬盘 > 选择转换后的文件
在虚拟机打开系统，通过df -h查看发现，根目录还是原样
![](http://ow3dy62zt.bkt.clouddn.com/IMG34.png)

## 使用LVM扩展空间
在虚拟机上操作
因为要修改现有分区，所以要用LVM
LVM（Logic Volume Manager）逻辑卷管理，像RedHat系的默认分区管理方式，是建立在硬盘分区之上，文件系统之下的逻辑层，用来解决在最初分区时未正确的评估和和分配分区容量，而造成系统分区不够用。

```
#查看当前系统分区情况
fdisk -l

#进行分区
fdisk /dev/sda

#重读分区表
partprobe

#将分区格式化为ext4格式
mkfs.ext4 /dev/sda3

#开始LVM操作,查看卷组名，即VG Name，我这里是centos
vgdisplay

#创建新物理卷，sda3是之前分区分配的
pvcreate /dev/sda3
#扩展到卷组
vgextend centos /dev/sda3

#查看根分区
lvdisplay
```
![](http://ow3dy62zt.bkt.clouddn.com/IMG35.png)

`/dev/centos/root`就是根分区，也是我们要扩展的分区
```
#扩展到容量逻辑分区
lvextend /dev/centos/root /dev/sda3

#刷新逻辑分区容量
resize2fs /dev/centos/root
```
报错
![](http://ow3dy62zt.bkt.clouddn.com/IMG36.png)
因为我的centos7的某些分区用的是xfs的文件系统（使用df -T查看即可知道）
使用`xfs_growfs /dev/centos/root`代替`resize2fs /dev/centos/root`

```
#查看
df -h
```



参考：

* 扩充根目录空间：https://www.awaimai.com/1194.html
* 添加新的挂载点：https://blog.csdn.net/ganshuyu/article/details/17954733
* http://www.imooc.com/qadetail/100002