---
title: 【Linux】Linux权限管理06 chattr、lsattr
tags: Linux
abbrlink: 2ce6014e
date: 2018-04-27 15:54:28
---

## 作用
禁止修改某些重要的系统文件


<br/>  

## 使用条件

* 所支持的文件系统包括：ext2、ext3、ext4和xfs
* 一般要求内核版本不低于2.2(查看版本的命令如下：`uname -a`、` lsb_release -a`
* 不能保护 `/ `、`/tmp` 、`/dev`、` /var`目录
* `chattr `只能由root用户使用

<br />

## chattr
类似于`chmod`, `chmod`只是改变文件的读写、执行权限，更底层的属性控制是由`chattr`来改变的.
```
chattr [+-=] [选项] 文件或目录名

* +：增加权限
* -：减少权限 
* =：等于某权限
* a：即append
    * 如果对文件设置a属性，那么只能在文件中增加数据，不能删除也不能修改数据（不能使用vi，因为不能判断是增加还是修改，可以使用echo）
    * 如果对目录设置a属性，那么只允许在目录中建立和修改文件，但是不允许删除
* i：即insert
    * 如果对文件设置i属性，那么不允许对文件进行删除、改名、设定链接关系，同时不能写入或新增内容
    * 如果对目录设置i属性，那么只能修改目录下文件的数据，不允许建立和删除文件

chattr +a abc
chattr +i abc
```
<br >

## lsattr
查看文件系统属性
```
lsattr [选项] [文件名]

* -a：列出目录下的所有文件，包括隐藏文件
* -d：查看本目录自身的权限
```

<br/>


参考：

* http://www.cnblogs.com/Jimmy1988/p/7265816.html
* https://www.imooc.com/video/9667