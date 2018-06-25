---
title: Shell条件判断 case、for、while、until
tags: Shell
abbrlink: 2d466a59
date: 2018-05-24 22:48:25
---

## 多分支语句case
```
case $变量名 in
    "值1")
        如果变量值等于值1，执行程序1
        ;;
    "值2")
        如果变量值等于值2，执行程序2
        ;;
    *）
        如果变量值都不是以上值，则执行此程序
        ;;
esac
```

与if多分支最大区别是，case语句只能判断一种条件关系，而if语句可以判断多种条件关系

```
#!/bin/bash  
name='basename $0 .sh'  
case $1 in  
    s|start) echo "start..."
    ;;
    stop) echo "stop ..."
    ;;
    reload)echo "reload..."
    ;;
    *)echo "Usage: $name [start|stop|reload]"
    exit 1
    ;;
esac
```

| 分割多个模式，相当于 or


## for

```
for 变量 in 值1 值2 值3... 
    do
        程序
    done

for ((初始值;循环控制条件;变量变化))
    do
        程序
    done
```
in后面跟多少值，for循环就循环多少次，每次循环依次把值赋给变量，直到后面的值全都运行一遍。
可以将要操作的数据内容放在一个文件中，利用 for i in $(cat 文件名)来避免手动输入。也可以将内容赋给变量，利用for i in $val 来避免手动输入。

```
#!/bin/bash
#从1加到100

s=0
for ((i=1;i<=100;i=i+1))
    do
        s=$(($s+$i))
    done
#没有i++
```


## while循环
```
while [ 条件判断式 ]
    do
          程序
    done
```

实例：从1加到100
```
#!/bin/bash

sum=0
i=1
while [ $i -le 100 ]
    do
        sum=$(( $sum+$i ))
        i=$(( $i+1 ))
    done
echo "sum is : $sum"
```













http://wiki.jikexueyuan.com/project/shell-learning/case-statements.html