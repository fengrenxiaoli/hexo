---
title: GitLab+sphinx+Readthedocs
abbrlink: 89c53863
date: 2018-06-06 09:54:05
tags:
---

利用GitLab、sphinx、Readthedocs可以制作文档和博客，利用这种方式制作出来的博客更像一本书，能够结构化展示文章，比较适合笔记类
Gitlab用来存储代码
sphinx用来写博客
Readthedocs用来展示


## 安装 Sphinx

```
pip install sphinx
```

## 创建工程
```
mkdir mybook
cd mybook
sphinx-quickstart
```
输入工程名、作者名、版本号，分离source和build目录`Separate source and build directories (y/N) [n]: y`

build目录 运行make命令后，生成的文件都在这个目录里面
source目录 放置文档的源文件

<!--more-->
## 配置
改主题
```
html_theme = 'sphinx_rtd_theme'
```


## 创建仓库
在gitlab上创建仓库

```
git init
git add .
git commit -m "sphinx start"
git remote add origin https://github.com/[yourusename]/[yourrepository].git
git push origin master
```


## 导入ReadtheDocs

注册ReadtheDocs账号，因为直接用gitlab账号登录，所以没有设置Webhooks
从https://readthedocs.org/dashboard/import/
导入git链接
在管理中设置Python 配置文件`source/conf.py`，Python interpreter`Cpython 3.x`，保存



## 目录结构

index.rst
```
.. toctree::
   :maxdepth: 2
   :caption: Contents:

   linux/index
   python/index
```

python/index.rst
```
Python
==================================

.. toctree::
   :maxdepth: 2
   :caption: Contents:
   :numbered:

   变量
   语句
```




## 本地查看 
命令行执行 
```
make html
```





参考：
https://www.jianshu.com/p/78e9e1b8553a
http://abnerzhao.com/2017/10/14/quickstart-wiki/
