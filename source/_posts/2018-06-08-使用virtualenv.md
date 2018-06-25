---
title: 使用virtualenv
tags: Python
abbrlink: d528f132
date: 2018-06-08 22:53:56
---


virtualenv用来为一个应用创建一套“隔离”的Python运行环境

```
pip3 install virtualenv
# 安装virtualenv

mkdir myproject
cd myproject/

virtualenv venv
source venv/bin/activate     
# 激活virtualenv

pip install flexx

deactivate          
# 退出当前的venv环境
```

<!--more-->


参考：
* https://www.kancloud.cn/thinkphp/python-guide/39429