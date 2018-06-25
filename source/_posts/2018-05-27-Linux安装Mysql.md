---
title: Linux安装Mysql
tags: Linux应用
abbrlink: 42bdef23
date: 2018-05-13 17:27:59
---

## CentOS通过yum安装


查看要对应的yum库，https://dev.mysql.com/downloads/repo/yum/
每个安装包下会有一个类似mysql80-community-release-el6-n.noarch.rpm，替换wget对应的链接

### 添加源
```
wget https://dev.mysql.com/get/mysql80-community-release-el6-n.noarch.rpm
rpm -Uvh mysql80-community-release-el6-n.noarch.rpm
```


### 选择mysql安装的版本
如果只需要安装最新版mysql，可以跳过这一步
```
#列出可以安装的版本
yum repolist all | grep mysql
```
可以通过yum-config-manager选择安装的版本
```
#安装yum-config-manager，通过yum provides yum-config-manager可以查到在yum-utils包里
yum install -y yum-utils
#禁用最新的版本，开启需要的版本
yum-config-manager --disable mysql80-community
yum-config-manager --enable mysql57-community
```

也可以直接配置/etc/yum.repos.d/mysql-community.repo
将需要使用的版本的enabled设为1，将现在激活的版本设为0
```
[mysql80-community]
name=MySQL 8.0 Community Server
baseurl=http://repo.mysql.com/yum/mysql-8.0-community/el/6/$basearch/
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-mysql
```
验证
```
yum repolist enabled | grep mysql
```


### 安装
默认选择安装最新的MySQL版本的MySQL，会安装mysql-community-server、client、libs、common
```
yum install mysql-server
```

### 启动
```
systemctl start mysqld
systemctl status mysqld
```


### 初始化
因为版本不同，可能第一次登录没有密码，可以先查看以下文件，为空则没有密码，不为空则使用列出的密码
```
grep 'temporary password' /var/log/mysqld.log
```

使用此命令运行安全脚本，设置root密码，删除匿名用户，禁止远程root用户登录，删除测试数据库并对其进行访问，并重新加载权限表
MySQL的 validate_password 插件默认安装。这将要求密码至少包含一个大写字母，一个小写字母，一个数字和一个特殊字符，并且总密码长度至少为8个字符。
```
mysql_secure_installation
```




## Ubuntu安装MySQL

```
sudo apt install mysql-server mysql-client libmysqlclient-cev
```


<!-- ## 通过二进制包安装
https://dev.mysql.com/doc/refman/5.5/en/binary-installation.html
https://hk.saowen.com/a/36cde9c26d5c84e1eee87c14cf18291631654dc38cf45f6d34cafc3576d8ca5a
https://hk.saowen.com/a/36cde9c26d5c84e1eee87c14cf18291631654dc38cf45f6d34cafc3576d8ca5a
```
##卸载残留的mysql文件
rpm -qa | grep mysql
rpm -ev 上一条语句查询的包
#mariadb-lib包用yum remove mysql-libs

#下载，这里使用
MySQL-5.5.60-1.el7.x86_64.rpm-bundle.tar

#解压
tar -xvf mysql-版本号

#安装
rpm -ivf ...
``` -->



## 配置mysql

### 编码
```
#mysql配置文件为/etc/my.cnf

最后加上编码配置
default-character-set=utf8
```

### 设置密码
```
>SET PASSWORD FOR 'jeffrey'@'localhost' = 'auth_string';
#为当前用户设置密码
>SET PASSWORD = 'auth_string';
```

### 新增用户
```
>create database testdb;
>create  user  'testuser'@'localhost'  identified  by  'password' ;
>grant  all  on testdb.* to 'testuser'@'localhost';
```


### 远程连接设置
```
#把在所有数据库的所有表的所有权限赋值给位于所有IP地址的root用户
> grant all privileges on *.* to root@'%'identified by 'password';

```



<!-- ## 源码包安装 
内存小的不要用这种方法了，make卡在了48%哈哈。。

```
#安装依赖，依赖包并不是一定提前知道的，是根据安装的报错信息逐渐添加的，看官方文档的当我没说。。
#反正全安好就行，m4和perl应该是安装bison的时候被安装了
yum install cmake ncurses-devel gcc gcc-c++ bison bison-devel
#下载安装boost c++库，https://sourceforge.net/projects/boost/files/boost/1.59.0/
tar -zxvf boost_1_59_0.tar.gz
cp -r boost_1_59_0/ /usr/local/

#获取源码，http://dev.mysql.com/downloads/mysql/，这里选择MySQL-5.7.22.tar.gz
wget https://dev.mysql.com/get/MySQL-5.7.22.tar.gz
tar -zxvf MySQL-5.7.22.tar.gz
cd MySQL-5.7.22
#cmake出错重新cmake之前需要删除CMakeCache.txt文件
rm -rf CMakeCache.txt
cmake . -DWITH_BOOST=/usr/local/boost_1_59_0

make
#内存不足，设置交换内存或者增加虚拟机内存

#MySQL的默认安装目录为/usr/local/mysql，DESTDIR参数指定安装目录
make install DESTDIR="/opt/mysql"

其他配置参考https://itbilu.com/database/mysql/VJVOut01M.html
``` -->


参考：
* https://dev.mysql.com/doc/mysql-yum-repo-quick-guide/en/
* https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-centos-7
* 安装mariadb：https://www.digitalocean.com/community/tutorials/how-to-install-mariadb-on-centos-7
* https://blog.gtwang.org/linux/centos-7-install-mariadb-mysql-server-tutorial/
* http://www.cnblogs.com/starof/p/4680083.html
* https://itbilu.com/database/mysql/VJVOut01M.html