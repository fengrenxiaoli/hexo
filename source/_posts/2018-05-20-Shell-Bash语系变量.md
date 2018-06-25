---
title: Shell变量 Bash语系变量
tags: Shell
abbrlink: 35dd1cd2
date: 2018-05-19 18:03:47
---


## 当前语系查询
```
locale
#查询当前系统语系
#LANG:定义系统主语系的变量
#LC_ALL:定义整体语系的变量
```

## 语系变量LANG
```
echo $LANG
#查看系统当前语系
locale -a | more
#查看Linux支持的所有语系

```

## 查询系统默认语系
```
cat /etc/sysconfig/i18n
#下次开机以后的系统环境
```


## 设置当前语系
```
LANG=zh_CN.UTF-8  
#切换成中文

LANG=en_US.UTF-8
```
Linux支持中文的前提条件是正确安装中文字体和中文语系
* 如果有图形界面，可以正确使用支持中文显示
* 如果使用第三方远程工具，只要语系设定正确，可以支持中文显示
* 如果使用纯字符界面，必须使用第三方插件（如zhcon等），即使设置LANG变量也没用
