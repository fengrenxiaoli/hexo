---
title: 'awk: run time error: not enough arguments passed to printf(5%)'
tags: Shell
abbrlink: 3608f886
date: 2018-05-21 08:25:38
---

## 问题

使用`awk '{printf $1}' `时，系统提示出错，如下
```
awk: run time error: not enough arguments passed to printf("5%")
        FILENAME="b" FNR=1 NR=1
```


## 解决
因为要输出的内容里`5%` 包含`%`，printf认为这是格式语句，所以更改printf的使用方式
使用如下格式：
```
awk '{printf("%s",$1)}'
```