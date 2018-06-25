---
title: Linux 字符处理命令sort、wc、tr、uniq
tags: Linux
abbrlink: 616ff62c
date: 2018-05-05 12:36:59
---

## sort 排序命令
对文本中的行进行排序
```
sort [OPTION]... [FILE]...
选项：
-f              忽略大小写
-n              以数值型进行排序，默认使用字符串型排序
-r              反向排序
-t DELIMITER    指定分隔符,默认是制表符
-k n[,m]        按照指定的字段范围排序。从第n字段开始，m字段结束(默认到行尾)
-u              uniq，排序后去重

sort /etc/passwd   
# 按照字母顺序a-z排列文件内容
sort -r /etc/passwd   
# 反向排序，即按z-a顺序排列文件内容
sort -t" " -k3 users
# 使用空格把users文件分割 使用第3列排序
sort -n -t ":" -k 3 /etc/passwd   
# 以数值型进行排序，指定分隔符为“:”，使用第3列排序
```


## wc 统计命令(word count)
```
wc [选项] 文件名
选项:
-l      只统计行数
-w      只统计单词数
-m      只统计字符数
-c      只统计字节数

# 若不加参数，则列出行数 词数 字符数
```
wc执行后 输入 ctrl+d结束 会统计输入行数 单词数 字符数

        

## tr 字符替换
```
tr [选项] ... SET1 [SET2]
从键盘输入转换或删除字符
-d  删除 
```

## uniq 去重命令 
对文本中的**连续的重复**行进行操作

如果不连续就不无法使用该命令，所以可以先用`sort`排序，可以看出哪些重复了

```
uniq [OPTION]... [FILE]...
-c: 显示每行重复出现的次数
-d: 仅显示重复过的行
-u: 仅显示不曾重复的行


uniq uniqtest.txt
# 连续重复的行只显示一行
uniq -d uniqtest.txt
# 只显示重复的行
uniq -D uniqtest.txt
# 显示全部重复的行
uniq -c uniqtest.txt
# 显示重复次数
```

cut命令：文本分割
    



## cut 字符截取命令
grep行提取，cut列提取
```
cut [OPTION]... [FILE]... 
选项 
-f 列号         提取第几列，从1开始
-d 分隔符       按照指定分隔符分割列，默认为tab

cut -f 1 /etc/passwd
cut -f 1-3 /etc/passwd
cut -f 1,3 /etc/passwd
cut -f 1-3,7 /etc/passwd
cut -d: -f 1,3-5 /etc/passwd
# 使用冒号分割passwd文件，显示第1、3至5列
cut -de -f 1 users
# 使用e字符分割文件 取第1列
cut -d' ' -f 1-2 users >user2
# 使用空格分割文件，显示1-2列，将标准输出重定向到新的文件

grep "bin/bash" /etc/passwd     可以登录的用户
grep "bin/bash" /etc/passwd | grep -v "root"    排除root的可登录用户
grep "bin/bash" /etc/passwd | grep -v "root" | cut -f 1 -d ":" 提取非root登录用户用户名
```
     
        
        
        
        
          

### cut命令的局限性

用cut截取比较规律的文件，用默认制表符或其他符号作为分隔符，可以方便截取，如果是用空格或多个空格做分隔符，就会有问题
```
df -h | cut -d " " -f 1,3
无法正确分隔多个空格，只能以一个空格分隔
```

## 例子

以冒号分隔，取出/etc/passwd文件的第6至第10行，并将这些信息按第3个字段的数值大小进行排序；最后仅显示的各自的第1个字段；
```
head -n 10 /etc/passwd | tail -n 4 | sort -n -t ":" -k 3 | cut -f 1 -d ":"
```
