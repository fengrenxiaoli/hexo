---
title: 【Linux】Linux网络管理05 虚拟机网络配置详解(NAT、桥接、Hostonly)
tags: Linux
abbrlink: 67c33d98
date: 2018-05-02 16:56:23
---


无论是vmware还是virtual box虚拟机软件，一般来说，虚拟机有三种网络模式:
1. Bridged Adapter（桥接模式）
2. NAT（网络地址转换模式）
3. Host-only Adapter（主机模式）

virtual box中还有lInternal


虚拟网卡在虚拟机安装好之后，会自动添加两张网卡(VMnet1和VMnet8)，VMnet1用Host-only网络连接，VMnet8用NAT方式的网络连接，原先的VMnet0用桥接网络连接。

![](/img/IMG23.png)


## 桥接
桥接网络是指宿主物理网卡和虚拟网卡通过VMnet0虚拟交换机进行桥接，物理网卡和虚拟网卡在拓扑图上处于**同等地位**，那么物理网卡和虚拟网卡就相当于处于同一个网段，虚拟交换机就相当于一台现实网络中的交换机，所以两个网卡的IP地址也要设置为同一网段。会占用内网IP

vmnet0实际上就是一个虚拟的网桥(2层交换机)，这个网桥有若干个接口，一个端口用于连接你的Host主机，其余端口可以用于连接虚拟机，他们的位置是对等的，谁也不是谁的网关。
![](/img/IMG24.png)


主机A上的两个虚拟机1和虚拟机2，和主机A、B同处于一个网段，能够相互通信
![](/img/IMG25.png)


虚拟机网卡配置，虚拟机上网需要IP/子网掩码/DNS/网关
```
DEVICE="eth0"
BOOTPROTO=“static" #设置静态ip,动态为dhcp
IPADDR="192.168.1.3"
GATEWAY="192.168.1.1"
HWADDR="08:00:27:C7:1B:22"
DNS1="8.8.8.8"
NETMASK="255.255.255.0"
ONBOOT="yes"
```
CentOS 7中ONBOOT默认为NO，需要打开

### 应用场景：
虚拟机要求可以上网，且虚拟机完全模拟一台实体机


<br/>
## NAT
NAT模式中，就是让虚拟机借助NAT(网络地址转换)功能，通过宿主机器所在的网络来访问公网。宿主能够联网，虚拟机也能联网(其他主机)。宿主没有联网，虚拟机也不能联网

vmnet1也是一个虚拟的交换机，交换机的一个 端口连接到你的Host上，另外一个端口连接到虚拟的DHCP服务器上（实际上是vmware的一个组件），另外剩下的端口就是连虚拟机了，主机A和虚拟机1和2能相互通信，虚拟机1和2能访问主机B和外网，主机B不能访问虚拟机1和2，虚拟机1和2能相互通信
![](/img/IMG27.png)

虚拟机的配置:
```
DEVICE="eth0"
BOOTPROTO=“static" #设置静态ip,动态为dhcp
IPADDR="10.0.2.5"
GATEWAY="10.0.2.1"
HWADDR="08:00:27:C7:1B:22"
DNS1="10.0.2.1"
NETMASK="255.255.255.0"
ONBOOT="yes"
```
### 应用场景：
虚拟机只要求可以上网，无其它特殊要求，满足最一般需求

<br />

## Host-Only
**所有的虚拟系统是可以相互通信的，但虚拟系统和真实的网络是被隔离开的**

虚拟系统和宿主机器系统是可以相互通信的。
虚拟系统的TCP/IP配置信息(如IP地址、网关地址、DNS服务器等)，都是由VMnet1(host-only)虚拟网络的DHCP服务器来动态分配的。

主机和虚拟机之间的通信是通过 VMnet1虚拟网卡来实现的。虚拟机连接到VMnet1上，系统并不为其提供任何路由服务，因此虚拟机只能和宿主机进行通信，而不能连接到真正的网络上。


Host-Only的宗旨就是建立一个与外界隔绝的内部网络，来提高内网的安全性。这个功能或许对普通用户来说没有多大意义，但大型服务商会常常利用这个功能。如果你想为VMnet1网段提供路由功能，那就需要使用RRAS，而不能使用XP或2000的ICS，因为ICS会把内网的IP地址改为192.168.0.1，但虚拟机是不会给VMnet1虚拟网卡分配这个地址的，那么主机和虚拟机之间就不能通信了。


虚拟机1和2之间可以相互通信，主机A能和虚拟机1和2通信，虚拟机1和2不能和主机通信(需要设置)，虚拟机不能和B主机以及外网通信
![](/img/IMG26.png)

###  使用范围
如果你想利用VMWare创建一个与网内其他机器相隔离的虚拟系统，进行某些特殊的网络调试工作，可以选择host-only模式。


<br />

## 内网模式

内网模式，顾名思义就是内部网络模式：虚拟机与外网完全断开，只实现虚拟机于虚拟机之间的内部网络模式。

虚拟机与主机的关系：不能相互访问，彼此不属于同一个网络，无法相互访问。
虚拟机与网络中其他主机的关系：不能相互访问，理由同上。
虚拟机与虚拟机的关系：可以相互访问，前提是在设置网络时，两台虚拟机设置同一网络名称。如上配置图中，名称为intnet。






参考：
* http://www.cnblogs.com/ggjucheng/archive/2012/08/19/2646007.html
* https://www.jianshu.com/p/305f7384cfe9
* https://blog.csdn.net/bytxl/article/details/35569217
* https://blog.csdn.net/clevercode/article/details/45934233
* http://blog.51cto.com/wangchunhai/381225
* https://github.com/waltcow/blog/issues/21
* https://blog.csdn.net/guizaijianchic/article/details/72190394
