---
title: 抓包工具tcpdump、wireshark
tags: 计算机网络
abbrlink: 8a478097
date: 2018-06-21 14:44:38
---


在Linux下，当我们需要**抓取网络数据包分析**时，通常是使用工具**tcpdump**。但是，有时我们需要将抓取的数据包保存在一个文件中，已备以后分析。而tcpdump保存的文件是**二进制文件**，使用cat 和vim 都无法打开查看。此时我们采取的措施是，下载到本地使用**wireshark**界面网络分析工具进行网络包分析。

## tcpdump
需要管理员权限
```
tcpdump host www.baidu.com
tcpdump host www.baidu.com and port 80
tcpdump host www.baidu.com -w out.cap

# 经过eth1
tcpdump -i eth1 host 192.168.1.1
# 指定源地址，抓取主机发送的所有数据
tcpdump -i eth1 src host 192.168.1.1
# 指定目的地址，抓取主机接收的所有数据
tcpdump -i eth1 dst host 192.168.1.1

# 经过eth1
tcpdump -i eth1 port 25
# 指定源端口
tcpdump -i eth1 src port 25
# 指定目的端口
tcpdump -i eth1 dst port 25

tcpdump -c100

tcpdump host 10.37.63.255 and (10.37.63.61 or 10.37.63.95)
tcpdump host 10.37.63.255 and !10.37.63.61


# host 主机地址，后面可以带具体的IP或者地址
# port 端口号
# -w 保存到文件
# -r 读取保存的文件
# src源地址
# dst目标地址
# -c 指定捕获的报文数量
# -i 指定接口 ifconfig -a查看有哪些接口
```








## wireshark

wireshark是一个图形化的工具

![](/img/IMG138.png)
![](/img/IMG139.png)
![](/img/IMG140.png)







补充：

- https://linuxwiki.github.io/NetTools/tcpdump.html
- https://wizardforcel.gitbooks.io/network-basic/content/16.html
- https://www.jianshu.com/p/8d9accf1d2f1
- 
- https://www.wireshark.org/docs/wsug_html_chunked/index.html
- https://wizardforcel.gitbooks.io/wireshark-manual/content/1.html
- 
- http://blog.51cto.com/zhaoyuqiang/1575315