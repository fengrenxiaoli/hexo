---
title: 【Hexo】使用hexo d无法上传问题解决办法
tags: hexo
abbrlink: 453dda43
date: 2016-10-09 19:26:21
---
### 问题1 ###
在使用`hexo d`上传时，可能出现以下错误
```
ERROR Deployer not found:git
```
### 解决办法 ###
1.在博客目录下安装hexo-deployer-git
```
npm install hexo-deployer-git --save
```
2.在博客目录下的_config.yml文件中
```
deploy:
	type: git
	repository: https://github.com/fengrenxiaoli/fengrenxiaoli.github.io.git
	branch: master
```
**type使用git，冒号后面需要一个半角空格**


### 问题2 ###
使用`hexo d`中，出现：
```
bash: /dev/tty: No such device or address
error: failed to execute prompt script (exit code 1)
fatal: could not read Username for 'https://github.com': No error
```
### 解决办法 ###
1.不要使用Cygwin等cmd工具，使用自带的cmd工具（具体可以参考<https://github.com/atom/atom/issues/8984>

2.使用ssh(具体格式在github中有)代替_config.yml文件中的deploy


如果出现
```
INFO  Deploy done: git
```
则说明上传成功


