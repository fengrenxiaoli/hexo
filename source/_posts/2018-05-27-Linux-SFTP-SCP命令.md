---
title: Linux文件传输-SFTP/SCP/FTP/lrzsz命令
tags: Linux应用
abbrlink: b2596d13
date: 2018-05-18 10:20:11
---

# FTP

## 连接ftp服务器
```
ftp [hostname| [ip-address]
```

## 列出文件列表以及切换目录
```
ls
cd
```


## 下载文件

下载文件通常用get和mget这两条命令。

```
#get [remote-file] [local-file]
#获取远程服务器上/usr/your/1.htm
ftp> get /usr/your/1.htm 1.htm

mget [remote-files]
#从远端主机接收一批文件至本地主机
#获取服务器上/usr/your/下的所有文件
cd /usr/your/
mget *.*

#显示下载进度
ftp> hash
```



## 上传文件
注意上传命令需要指定目标文件名
```
put local-file [remote-file]
#将本地一个文件传送至远端主机中。
#把本地的1.htm传送到远端主机/usr/your,并改名为2.htm
ftp> put 1.htm /usr/your/2.htm

mput local-files
#将本地主机中一批文件传送至远端主机。
#把本地当前目录下所有html文件上传到服务器/usr/your/ 下
ftp> cd /usr/your
ftp> mput *.htm
```

## 断开连接
```
bye：中断与服务器的连接
ftp> bye
```

## 改变传输模式
ftp的传输模式有ascii模式和二进制模式
直接输入ascii则设置传输模式为ascii模式
直接输入binary则设置传输模式为binary模式

```
ftp> ascii
ftp> binary
```

# SFTP

SFTP是安全文件传送协议，可以为传输文件提供一种安全的网络的加密方法。

SFTP 与 FTP 有着几乎一样的语法和功能。SFTP 为 SSH的其中一部分，所以说 SFTP 就是通过SSH端口（默认 22端口）和 Linux 用户和密码登陆的（例如 root 账号）。SFTP 使用加密传输认证信息和传输的数据，所以使用SFTP是非常安全的。但是由于这种传输方式使用了加密/解密技术，所以传输效率比普通的FTP要低得多。

使用SFTP并不需要在服务器上做任何配置，只需要找个SFTP客户端，然后知道SSH端口、服务器用户名+密码即可。

```
# 连接到SFTP
sftp tecmint@27.48.137.6

# 获得帮助
?
help

# 检查本地工作目录
lpwd
# 检查远程工作目录
pwd

# 列出本地文件
lls
# 列出远程文件
ls

# 上传单个或多个文件
put local.profile
# 上传多个文件
mput * .xls

# 下载单个或多个文件
get SettlementReport_1-10th.xls
# 下载多个文件
mget * .xls

# 切换本地目录
lcd Documents
# 切换远程目录
cd test

# 创建目录
mkdir test
lmkdir test

# 删除目录，该目录必须为空
rm Report.xls
rmdir sub1

# 退出SFTP
!
exit
```

# SCP
使用SSH协议来传输文件的

* SCP比较简单，是轻量级的，SFTP的功能则比较多
* SCP的速度较快
* SFTP在文件传输过程中中断的话，连接后还可以继续传输，但SCP不行

```
scp [-r] 用户名@ip:文件路径 本地路径
# 网络复制命令, 下载文件 或加-r下载文件夹

scp [-r] 本地文件 用户名@ip:上传路径
# 网络复制命令, 上传文件 或加-r上传文件夹
# 此为linux 与 linux之间进行文件传输的最简单方式

```

# lrzsz
sz/rz 并不是Linux标准命令工具，有些Linux发行版本如Ubuntu会自带，有些可能没有，需要自己安装

安装lrzsz 
```
yum -y install lrzsz 
```
```
#上传文件，执行命令rz，会跳出文件选择窗口，选择好文件，点击确认即可。
rz
```

```
#下载文件，执行命令sz
sz
```

参考：
* https://www.tecmint.com/sftp-command-examples/
* https://www.jscape.com/blog/scp-vs-sftp
