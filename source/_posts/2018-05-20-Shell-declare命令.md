---
title: Shell运算符 declare命令
tags: Shell
abbrlink: 2b659642
date: 2018-05-19 22:02:08
---



## declare命令
```
declare [+/-][选项] 变量名 
#declare命令用来声明shell的变量类型，因为shell变量默认都是字符串型
选项：
-：用于给变量设定类型属性
+：用于取消变量的类型属性
-a：将变量声明为数组型
-i：将变量声明为整型
-x：将变量声明为环境变量
-r：将变量声明为只读变量
-p：显示指定变量被声明的类型

aa=11 
bb=22
declare -i cc=$aa+$bb
declare -p c  #查看变量cc的类型
#声明变量cc的类型是整数型，它的值是aa和bb的和
```
<br>


## 声明数组变量
```
#定义数组
#数组的定义不需要declare命令也可以，直接使用movie[i]=value
movie[0]=zp
movie[1]=tp
declare -a movie[2]=live

#查看数组
echo ${movie}   #输出数组下标为0的变量值
echo ${movie[2]} 
echo ${movie[*]} #输出数组的全部值
```
<br>

## 声明环境变量
```
declare -x test=123
#和export作用类似，export命令实际过程是调用declare命令
```

<br>

## 声明变量只读属性
```
declare -r test
#给test赋予只读属性，赋予后不能修改该变量，不能删除，甚至不能取消只读属性
#临时生效，重启无效
```

<br>

## 查询变量的属性
```
declare -p
#列出系统中所有变量的类型

declare -p 变量名
#查询指定变量的属性
```