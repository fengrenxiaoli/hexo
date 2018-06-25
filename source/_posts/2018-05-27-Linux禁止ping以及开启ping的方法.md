---
title: Linux禁止ping以及开启ping的方法
tags: Linux应用
abbrlink: a436331
date: 2018-05-16 19:56:24
---


Linux默认是允许Ping响应的，系统是否允许Ping由2个因素决定的：A、内核参数，B、防火墙，需要2个因素同时允许才能允许Ping，2个因素有任意一个禁Ping就无法Ping。

 

具体的配置方法如下：

## 内核参数设置

#### 允许ping设置
```
#临时允许PING操作的命令
echo 0 >/proc/sys/net/ipv4/icmp_echo_ignore_all

#永久允许ping配置方法
#/etc/sysctl.conf设置
net.ipv4.icmp_echo_ignore_all=0
#使新配置生效
sysctl -p
```


#### 禁止Ping设置     
```
#临时禁止PING的命令
echo 1 > /proc/sys/net/ipv4/icmp_echo_ignore_all     

#永久禁止ping配置方法
#/etc/sysctl.conf设置
net.ipv4.icmp_echo_ignore_all=1
#使新配置生效
sysctl -p
```



## 防火墙设置
注：此处的方法的前提是内核配置是默认值，也就是没有禁止Ping

这里以Iptables防火墙为例，其他防火墙操作方法可参考防火墙的官方文档。

#### 允许ping设置      
```
iptables -A INPUT -p icmp --icmp-type echo-request -j ACCEPT
iptables -A OUTPUT -p icmp --icmp-type echo-reply -j ACCEPT
#或者也可以临时停止防火墙操作的
service iptables stop
```

#### 禁止ping设置
```
iptables -A INPUT -p icmp --icmp-type 8 -s 0/0 -j DROP
```


参考：
* https://www.cnblogs.com/chenshoubiao/p/4781016.html