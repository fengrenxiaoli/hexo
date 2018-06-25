---
title: Shell Bash别名和快捷键
tags: Shell
abbrlink: c84d750c
date: 2018-05-19 10:41:19
---

## 别名
```
#增加新的别名前要查看是否被占用
type 别名

alias
#查看系统当中默认已经生效的别名

alias ls='ls --color=auto'
#临时设定别名
#不能有空格

vi ~/.bashrc
vi /root/.bashrc
vi /etc/.bashrc
#写入环境变量配置文件，系统再次重启后永久生效
source .bashrc
#使当前环境变量设置立即生效，不需要系统重启

unalias ls
#删除别名


\cp
#不执行别名，执行命令本身
```

命令生效顺序
1. 第一顺位执行绝对路径或者相对路径的命令
2. 第二顺位执行别名
3. 第三顺位执行Bash的内部命令
4. 第四顺位执行按照$PATH环境变量设置定义的目录顺序的第一个命令

<br>

## 快捷键
ctrl + c 强制终止
ctrl + l 清屏相当于clear
ctrl + a 光标快速回到行首
ctrl + e 光标快速去到行尾
ctrl + u 从光标所在位置删除到行首
ctrl + k 从光标所在位置删除到行尾
ctrl + z 把命令放入后台--，暂停
ctrl + r 历史命令搜索