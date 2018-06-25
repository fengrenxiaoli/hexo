---
title: Shell正则表达式 格式化输出命令printf
tags: Shell
abbrlink: 50c951db
date: 2018-05-20 17:49:39
---


```
printf "输出类型输出格式" 输出内容

输出类型：
%ns：输出字符串。n是数字，指代输出几个字符
%ni：输出整数。n是数字，指代输出几个数字
%m.nf：输出浮点数。m和n是数字，指代输出的整数位数和小数位数。如%8.2f代表共输出8位数，其中2是小数位数，6位是整数

输出格式：
\a：输出警告声音
\b：输出退格键，也就是Backspace键
\f：清空屏幕
\n：换行
\r：回车，也就是Enter键
\t：水平输出退格键，也就是Tab键
\v：垂直输出退格键，也就是Tab键

printf %s 1 2 3 4 5 6           123456
printf %s %s %s 1 2 3 4 5 6     %s%s123456      后两个%s被当作输入
printf '%s %s %s' 1 2 3 4 5 6   1 2 34 5 6      
printf '%s\t%s\t%s\n' 1 2 3 4 5 6
1 	2	3
4	5	6
```

使用printf输出命令，必须明确指出所有的格式
如果想要使用printf读取文件中的内容就需要：
```
printf '%s' $(cat student.txt)  
#不调整输出格式，文本内的内容输出到一行

printf '%s\t%s\t%s\t%s\n' $(cat student.txt) 
#调整输出格式，根据文本内容进行调整
```

print在输出之后会在自动加入换行符，但Linux系统中默认没有print命令
printf是标准格式输出命令，并不会自动加入换行符，如需换行，需要手动加入换行符
printf "%s\n" a