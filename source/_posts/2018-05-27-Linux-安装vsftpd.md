---
title: Linux 安装vsftpd
tags: Linux应用
abbrlink: c81fedea
date: 2018-05-07 15:15:39
---

> 记，这篇文章实际是相当复杂的，主要原因是出于安全考虑增加了很多内容，如果只是局域网内使用不需要如此复杂，只需要安装、关闭匿名、指定目录和用户，参考https://fengrenxiaoli.github.io/2018/05/18/%E5%B1%80%E5%9F%9F%E7%BD%91%E5%AE%89%E8%A3%85ftp/

vsftpd 的全称是”very secure FTP daemon“，是一款在Linux发行版中最受推崇的FTP服务器程序。特点是小巧轻快，安全易用。


总结起来只有几步
* 安装
* 配置文件
* 防火墙
* 设置ftp用户

一、安装FTP服务器
```
#查看是否已经安装vsftpd
rpm -qa | grep vsftpd

yum -y install vsftpd
```

二、手动启动，并使其能够从下次系统启动时自动启动
```
systemctl start vsftpd
systemctl enable vsftpd
#chkconfig vsftpd on
```

三、允许从外部系统访问FTP服务
```
firewall-cmd --permanent --zone=public  --add-port=21/tcp
firewall-cmd --permanent --zone=public  --add-service=ftp
firewall-cmd --reload
```

四、备份配置文件
```
cp /etc/vsftpd/vsftpd.conf /etc/vsftpd/vsftpd.conf.bak
cp /etc/vsftpd.userlist /etc/vsftpd.userlist.bak
```

五、配置`/etc/vsftpd/vsftpd.conf`
没有的请自行添加
```
anonymous_enable = NO＃禁用匿名登录
local_enable = YES＃允许本地登录
write_enable = YES＃启用更改文件系统的FTP命令
local_umask = 022＃用于本地用户创建文件的umask值
dirmessage_enable = YES＃用户首次进入新目录时启用显示消息
xferlog_enable = YES＃会保存一个日志文件，详细说明上传和下载
connect_from_port_20 = YES＃使用服务器机器上的端口20（ftp-data）进行端口连接
xferlog_std_format = YES＃保持标准的日志文件格式
listen = NO＃阻止vsftpd在独立模式下运行
listen_ipv6 = YES＃vsftpd将监听IPv6套接字而不是IPv4套接字
pam_service_name = vsftpd＃vsftpd将使用的PAM服务的名称
userlist_enable = YES＃启用vsftpd加载用户名列表
tcp_wrappers = YES＃打开tcp
```

FTP基于用户列表文件允许/拒绝对用户的FTP访问
默认情况下，如果userlist_enable = YES  并且userlist_deny = YES，在`userlist_file=/etc/vsftpd/user_list`列出的用户都无法登录访问FTP。

如果userlist_deny = NO，这意味着只有`userlist_file = /etc/vsftpd/user_list`中明确列出的用户才能登录。

```
userlist_enable = YES ＃vsftpd将加载从userlist_file给出的用户名列表
userlist_file = /etc/vsftpd/user_list＃存储用户名
userlist_deny = NO
```

when users login to the FTP server, they are placed in a chroot’ed jail, this is the local root directory which will act as their home directory for the FTP session only.
当用户登录到FTP服务器时，他们被放置在chroot jail里，chroot jail是本地根目录，会作为FTP会话的主目录

限制FTP用户到他们的主目录
```
chroot_local_user = YES 
allow_writeable_chroot = YES
```
`chroot_local_user = YES`意味着本地用户通过默认设置登录后，他们的主目录将被放置在chroot jail中

并且默认情况下，vsftpd不允许chroot jail目录因安全原因而可写，但是，我们可以使用选项`allow_writeable_chroot = YES`来覆盖此设置。

保存该文件并关闭它


六、配置SELinux
```
setsebool -P ftp_home_dir on
#会报错
```
set SELinux rule to allow FTP to read/write user’s home directory
```
semanage boolean -m ftpd_full_access --on
-bash: semanage: command not found
yum provides semanage
yum install policycoreutils-python
systemctl restart vsftpd
```

七、测试FTP服务器
创建一个FTP用户
```
useradd -d /home/ftptest -g ftp -s /sbin/nologin ftptest
passwd ftpuser
```

将用户添加到文件/etc/vsftpd/user_list中
```
echo ftptest >> /etc/vsftpd.userlist
cat /etc/vsftpd.userlist
```

分别测试匿名用户anonymous、root、ftptest

```
ftp 192.168.0.203
anonymous	530
root		530
ftptest		230

#root因为在/etc/ftpusers目录里所以无法登录，该目录为禁止登录的用户
#ftptest进入后，执行ls，可以测试是否为ftptest用户自己的主目录
```

八、配置不同的FTP用户主目录

使用`allow_writeable_chroot=YES`具有某些安全隐患，特别是在用户具有上载权限或shell访问权限的情况下。
只有当你确切地知道你在做什么时才激活这个选项。需要注意的是，这些安全性影响并不是vsftpd特有的，它们适用于所有提供将本地用户放入chroot jail的FTP守护进程。

注释掉之前的语句
```
＃allow_writeable_chroot = YES
```

为用户创建替代本地根目录的目录（ravi您的可能不同），并将所有用户的写入权限移除到此目录
```
mkdir /home/ftptest/ftp
chown nobody：nobody /home/ftptest/ftp
chmod a+w /home/ftptest/ftp
```

修改vsftpd配置文件
```
user_sub_token = $USER＃将用户名插入本地根目录
local_root = /home/$USER/ftp＃定义任何用户本地根目录
```


重启vsftpd


注意上传命令需要指定目标文件名
```
ftp put /path/to/local_file remote_file_name
```


九、开放端口
如果使用云服务器，还需要开放端口

FTP服务器使用被动模式，需要先进行配置
```
vi /etc/vsftpd/vsftpd.conf

pasv_enable=YES
pasv_min_port=40000
pasv_max_port=41000
```

这里使用阿里云服务器
打开控制台，云服务器ECS->安全组->配置规则
添加21和40000-41000



参考：
* 重点https://www.kancloud.cn/chandler/bc-linux/52710
* http://how2j.cn/k/deploy2linux/deploy2linux-openport/1604.html
* https://www.tecmint.com/install-ftp-server-in-centos-7/
* https://wsgzao.github.io/post/vsftpd/
* https://blog.csdn.net/aiynmimi/article/details/77012507











