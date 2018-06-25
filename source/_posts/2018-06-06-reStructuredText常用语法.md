---
title: reStructuredText常用语法
abbrlink: 1d81ed52
date: 2018-06-06 10:32:15
tags:
---


reStructuredText是一种标记语言，和markdown类似，但是能够提供比markdown更丰富的样式



## 标题
```
==========
一级标题
==========


二级标题
==========


三级标题
----------


四级标题
^^^^^^^^^^^
```


## 字体样式
```
*这里是强调内容*

`这里是引用内容`

**这里是粗体内容**

``这里是等宽文本``


上标
E = mc\ :sup:`2`

下标
H\ :sub:`2`\ O
```

<!--more-->

## 段落
```
| 这里是段落

  缩进的段落被视为引文。

| 这里也是段落

  缩进的段落被视为引文。

| 这里还是段落

  缩进的段落被视为引文。
```




## 代码
```
行内代码
``echo "Hello World!";``

代码块
在代码块的上一个段落后面加2个冒号，空一行后开始代码块，代码块要缩进
::

    hello world
    hello world
    hello world
```


## 列表

```
下级列表需要有空格缩进
无序列表
- jj
- kk
- jj

有序列表
1. ll
2. oo
3. pp



```


## 图片

```
.. image:: https://help.github.com/assets/images/site/favicon.ico
```



## 链接
```
外部引用
这篇文章来自我的Github,请参考 reference_。
.. _reference: https://github.com/SeayXu/

`SeayXu <https://github.com/SeayXu/>`_
```






## 表格
```
简单表格
=====  =====  ======
输入          输出
------------  ------
A      B      A or B
=====  =====  ======
False  False  False
True   False  True
False  True   True
True   True   True
=====  =====  ======


普通表格
+------------------------+------------+----------+----------+
| Header row, column 1   | Header 2   | Header 3 | Header 4 |
| (header rows optional) |            |          |          |
+========================+============+==========+==========+
| body row 1, column 1   | column 2   | column 3 | column 4 |
+------------------------+------------+----------+----------+
| body row 2             | Cells may span columns.          |
+------------------------+------------+---------------------+
| body row 3             | Cells may  | - Table cells       |
+------------------------+ span rows. | - contain           |
| body row 4             |            | - body elements.    |
+------------------------+------------+---------------------+


CSV表格
.. csv-table:: 表头
 :header: "Treat", "Quantity", "Description"
 :widths: 15, 10, 30

 "Albatross", 2.99, "On a stick!"
 "Crunchy Frog", 1.49, "If we took the bones out, it wouldn't be
 crunchy, now would it?"
 "Gannet Ripple", 1.99, "On a stick!"



列表表格
.. list-table:: 表头
  :widths: 15 10 30
  :header-rows: 1

  * - Treat
    - Quantity
    - Description
  * - Albatross
    - 2.99
    - On a stick!
  * - Crunchy Frog
    - 1.49
    - If we took the bones out, it wouldn't be
      crunchy, now would it?
  * - Gannet Ripple
    - 1.99
    - On a stick!
```

表格可以使用插件https://macplay.github.io/posts/shi-yong-vim-zai-markdown-ji-rst-wen-dang-zhong-chuang-jian-biao-ge/


参考：
* https://www.jianshu.com/p/f60e9be4781d
* https://3vshej.cn/rstSyntax/index.html
* https://www.jianshu.com/p/9b8c2e10e5e9
* http://docutils-zh-cn.readthedocs.io/zh_CN/latest/ref/rst/restructuredtext.html



