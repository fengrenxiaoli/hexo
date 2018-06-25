---
title: Linux安装Anaconda
tags: Python
abbrlink: 634c672f
date: 2018-06-14 09:38:50
---


Anaconda 是一种Python语言的包管理工具，用于进行大规模数据处理, 预测分析, 和科学计算, 致力于简化包的管理和部署。 Anaconda使用软件包管理系统Conda进行包管理。

实际上我使用Anaconda是为了避免Python2和Python3的冲突，我既想使用Python3又不想改变Python2原来的东西

官网：https://anaconda.org/

## 安装

下载：https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/
选择对应的Python版本和系统进行下载安装


```
bash Anaconda3-5.2.0-Linux-x86_64.sh
conda upgrade --all
```
<!--more-->
## 安装包管理
列出已经安装的包：`pip list`或`conda list`
安装新包：`pip install 包名`或`conda install 包名`
更新包： `conda update package_name`
升级所有包：`conda upgrade --all`
卸载包：`conda remove package_names`
搜索包：`conda search search_term`



## 管理环境
安装nb_conda，用于notebook自动关联nb_conda的环境`conda install cb_conda`
创建环境：`conda create -n env_name package_names[=ver]`
指定Python版本：`conda create -n py3 python=3.6`
可以切换不同的Python版本
使用环境：`activate env_name`
离开环境：`deactivate`
导出环境设置：`conda env export > environmentName.yaml` 或 `pip freeze > environmentName.txt`
导入环境设置：`conda env update -f=/path/environmentName.yaml` 或 `pip install -r /path/environmentName.txt`
导出和导入可以用于共享环境，在 GitHub 上共享代码时，最好同样创建环境文件并将其包括在代码库中。这能让其他人更轻松地安装你的代码的所有依赖项。
列出环境清单：`conda env list`
删除环境： `conda env remove -n env_name`


参考：
- https://zh.wikipedia.org/wiki/Anaconda_(Python%E5%8F%91%E8%A1%8C%E7%89%88)