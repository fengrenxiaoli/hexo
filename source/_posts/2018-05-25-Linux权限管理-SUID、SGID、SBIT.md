---
title: 【Linux】Linux权限管理05 SUID、SGID、SBIT
tags: Linux
abbrlink: 271a6e14
date: 2018-04-27 15:54:05
---


## SUID(SetUID)
使用户临时具有**程序所有者**（比如root）的权限来执行该程序，比如调用`/usr/bin/passwd`命令修改自己的密码

SetUID(或者 s 权限）：当一个具有执行权限的文件设置SetUID权限后，用户执行这个文件时将以文件所有者的身份执行。passwd命令具有SetUID权限，所有者为root（Linux中的命令默认所有者都是root），也就是说当普通用户使用passwd更改自己密码的时候，那一瞬间突然 “灵魂附体” 了，实际在以passwd命令所有者root的身份在执行，root当然可以将密码写入`/etc/shadow`文件，命令执行完成后该身份也随之消失。
当然用户的passwd命令不能修改其他用户的密码，只能输入`passwd`来修改自己的密码

#### 使用要求
* 只有可执行的二进制程序才可以设置SetUID
* 命令执行者必须对欲设置SetUID的文件具备可执行(x) 权限，没有x的文件会成为S，S不能正确使用，只有s可以正确使用
* 命令执行过程中，其它用户获取所有者的身份（灵魂附体）
* SetUID具有时间限制，即完成该程序执行后就消失
* 不能对目录使用

#### 命令
4代表SUID，s出现在文件所有者的x权限上

```
#设置SetUID
chmod 4755 文件名
chmod u+s 文件名（推荐，不影响其他权限）

#取消SetUID
chmod 0755 文件名
chmod u-s 文件名（推荐，不影响其他权限）
```
#### 例程
以普通用户执行
```
ll /usr/bin/touch
touch test1
sudo chmod u+s /usr/bin/touch
ll /usr/bin/touch
touch test2
ll test1 test2

#比较前后两次的属性差异
-rw-rw-r--. 1 niesh niesh 0 7月  30 17:40 test1
-rw-rw-r--. 1 root  niesh 0 7月  30 17:42 test2
```
可以看到，在设置了SetUID之后，新建文件的所有者为root了，说明在执行touch的时候，用户自动升级为了所有者

#### 危险性
设置SetUID是具备很大危险性的，主要是设置权限过大而引起的问题
我们需要定时查看系统中有哪些设置了SetUID权限
* 关键目录应严格控制写权限。比如 `/`、`/usr`
* 用户的密码设置要严格遵循密码三原则(#复杂性，易记忆性，时效性）
* 对系统中默认应该具有SetUID权限的文件做一个列表，然后定期检查有没有这之外的执行程序的命令文件被设置了SetUID

使用shell定期检查SetUID
```
#!/bin/bash

find / -perm -4000 -o -perm -2000 > /tmp/setuid.check
for i in $(cat /tmp/setuid.check)

do   
        grep $i /root/suid.log > /dev/null
                if [ "S?" !="0"]
                then
                    echo "$i isn't in listfile!" >> /root/suid_log_$(date+%F)
                fi
done
rm -rf /tmp/setuid.check
```
<br/>

## SGID(SetGID)
SetGID基本与SetUID相同，SetUID是设置所有者的权限，SGID为设置所属组的权限
区别点在于：SetGID也可以设置目录的相关SetGID权限

#### 作用
将用户所在组临时升级为某一个组，以执行只有该组才有相应权限进行的操作

#### 使用要求
* 针对文件：
    * 可执行的二进制文件
    * 命令执行者（即所属组）对该文件具备 x 权限
    * 命令执行者在执行程序的时候，组身份升级为该程序文件的属组
    * 权限只在执行过程中有效
* 针对目录：
    * 普通用户对目录具备r和x权限，才可以进入到该目录
    * 普通用户在此目录中的有效组会变成此目录的所属组
    * 如普通用户对该目录具备w权限，新建文件的所属组为该目录的所属组

#### 命令
2代表SGID，s出现在文件所属群组的x权限上
```
#设置SetGID
chmod 2xxx <file/dir-name>
chmod g+s <file/dir-name> （推荐）

#取消SetGID
chmod xxx <file/dir-name>
chmod g-s <file/dir-name>
```
#### 例程
我们此处以locate命令进行讨论：
locate查询命令，比find要快很多，为什么？因为其实搜索的数据库而非整个硬盘：
```
[root@niesh ~]# ll /usr/bin/locate
-rwx--s--x. 1 root slocate 40496 6月  10 2014 /usr/bin/locate

[root@niesh ~]# ll /var/lib/mlocate/mlocate.db
-rw-r-----. 1 root slocate 6306909 7月  30 19:15 /var/lib/mlocate/mlocate.db
```
我用普通用户进行locate查看：
```
[niesh@niesh root]$ locate mlocate.db
/usr/share/man/man5/mlocate.db.5.gz
```
去掉locate的s权限：
```
[root@niesh ~]# chmod g-s /usr/bin/locate
[root@niesh ~]# ll /usr/bin/locate
-rwx--x--x. 1 root slocate 40496 6月 10 2014 /usr/bin/locate

[niesh@niesh root]$ locate mlocate.db
locate: 无法执行 stat () `/var/lib/mlocate/mlocate.db': 权限不够
```
也就是：当执行locate命令时，普通用户niesh自动升级为slocate的组成员。

* /usr/bin/locate是可执行二进制程序，可以赋予SGID
* 执行用户niesh对/usr/bin/locate命令拥有执行权限
* 执行/usr/bin/locate命令时，组身份会升级为slocate组，而slocate组对/var/lib/mlocate/mlocate.db数据库拥有r权限，所以普通用户可以使用locate命令查询mlocate.db数据库
* 命令结束，niesh用户的组身份返回为niesh组


<br/>

## SBIT(Sticky BIT)
粘滞位

#### 作用
防止其他用户删除自己的文件，使用者在该目录下，仅自己与root才有权力删除新建的目录或文件

#### 使用要求
只对目录有效
普通用户对该目录有w和x权限
若没有粘滞位，则普通用户可以对目录下的文件/子目录进行删除操作（因为普通用户对目录具有w权限），包括其它用户建立的目录/文件；但若赋了SBIT，则普通用户只能删除自己创建的文件/目录，而不能删除不属于自己的文件/目录！

#### 命令
1代表SBIT，t出现在文件其他用户的x权限上
```
#设置SBIT
chmod 1xxx < dir-name >
chmod o+t < dir-name >

#取消SBIT
chmod xxx < dir-name >
chmod o-t < dir-name >
```
#### 例程
以/tmp为例：
查看/tmp的权限：
```
[niesh@niesh tmp]$ ll -d /tmp/
drwxrwxrwt. 8 root root 4096 7月 30 19:40 /tmp/
```
会看到，/tmp目录的权限other部分为rwt,这个t就是我们设置的粘滞位
接下来，我们用其它用户创建两个文件：
```
[Jimmy@niesh tmp]$ touch test-file
[Jimmy@niesh tmp]$ mkdir test-dir
[Jimmy@niesh tmp]$ ll
总用量 0
drwxrwxr-x. 2 Jimmy Jimmy 6 7月  30 19:44 test-dir
-rw-rw-r--. 1 root  Jimmy 0 7月  30 19:44 test-file
```
切换到另外一个用户niesh:
```
[niesh@niesh tmp]$ ll
总用量 0
drwxrwxr-x. 2 Jimmy Jimmy 6 7月  30 19:44 test-dir
-rw-rw-r--. 1 root  Jimmy 0 7月  30 19:44 test-file
在 niesh用户下，删除/tmp目录下的文件：

[niesh@niesh tmp]$ rm -rf test-dir/ test-file
rm: 无法删除"test-dir/": 不允许的操作  
无法删除！
```
然后，我们切换到root，去掉/tmp的粘滞位：
```
[niesh@niesh tmp]$ su -
密码：
上一次登录：日 7月 30 19:43:21 CST 2017pts/0 上
[root@niesh ~]# chmod o-t /tmp/
[root@niesh ~]# ll -d /tmp/
drwxrwxrwx. 9 root root 4096 7月  30 19:48 /tmp/
```
最后，切换到普通用户niesh，再次删除/tmp下的文件：
```
[niesh@niesh root]$ rm -rf /tmp/test-dir/ /tmp/test-file
[niesh@niesh root]$ ll /tmp/
总用量 0
```







参考：

* http://www.cnblogs.com/Jimmy1988/p/7260215.html












