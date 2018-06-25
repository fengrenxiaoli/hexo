---
title: XML命名空间和XML Schema
abbrlink: 7866b39f
date: 2017-08-13 17:14:19
tags:
---
```
2018年5月记，看完之后相当懵逼，，我是为了什么写这个的
```

## 概念

XML命名空间提供避免元素命名冲突的方法。标签可以放入命名空间中，不同的命名空间中的相同名称标签是不同的标签。




## 命名冲突
在 XML 中，元素名称是由开发者定义的，当两个不同的文档使用相同的元素名时，就会发生命名冲突。

这个 XML 携带 HTML 表格的信息：
```
<table>
<tr>
<td>Apples</td>
<td>Bananas</td>
</tr>
</table>
```
这个 XML 文档携带有关桌子的信息（一件家具）：
```
<table>
<name>African Coffee Table</name>
<width>80</width>
<length>120</length>
</table>
```
假如这两个 XML 文档被一起使用，由于两个文档都包含带有不同内容和定义的 <table> 元素，就会发生命名冲突。
XML 解析器无法确定如何处理这类冲突。




## 语法
在xml文件中，命名空间的定义如下：
```
<d:student xmlns:d="http://www.develop.com/student" >
```
其中 student 是命名空间的标签。`http://www.develop.com`是命名空间的标识。d是命名空间的前缀。





### 命名空间标签
由于命名空间采取元素属性的定义方式，所以需要一个标签。



### xmlns 属性
当在 XML 中使用前缀时，一个所谓的用于前缀的命名空间必须被定义。
命名空间是在元素的开始标签的 xmlns 属性中定义的。
xmlns:前缀="URI"。
当命名空间被定义在元素的开始标签中时，所有带有相同前缀的子元素都会与同一个命名空间相关联。
命名空间 URI 不会被解析器用于查找信息。
其目的是赋予命名空间一个惟一的名称。不过，很多公司常常会作为指针来使用命名空间指向实际存在的网页，这个网页包含关于命名空间的信息。




### 命名空间标识
命名空间标识是命名空间最重要的属性，重要到当输出一个命名空间时就直接转换为它的标识。标识有个规范的称呼:URI(统一资源定位符)。URI的最大特点是唯一性。如果不唯一就失去了辨识的意义。实际上相同URI不同的命名空间被看成同一个命名空间。

URI分为两种类型：
URL(统一资源定位器):
通俗的说URL就是网页地址。因为每个网页在internat上都是唯一的。

URN（统一资源名称)：
可以不使用网页地址而使用唯一名称来定义。如：
urn:2007-12-9/workgrop/xin/projiectname
或 urn:E7f73B13-05FE-44ec-81CE-F898C4A6CDB4
这个编号是在系统中注册的控件编号，因此是唯一的。

### 前缀
前缀用于在XML中作为URI的简化引用。因为URI太长了。如：
```
<d:student xmlns:d="http://www.develop.com/student">
<d:id>3235329</d:id>
<d:name>Jeff Smith</d:name>
</d:student>
```
使用前缀把标签放入对应的命名空间中。


有了命名空间区分后相同标签名可以不会被错误解析。实际上命名空间加上元素名叫做QName。QName有两个属性：uri和localName，分别获取命名空间名和本地名称。这个QName可以使用xml的name()方法得到。如上例子中的xml文件可以使用如下代码访问：
```
var ns:Namespace=xml.namespace();
var node:XMLList=xml.ns::id;
var qNameName=node.name();
trace(qName.uri);
trace(qName.localName);
```

命名空间不一点要定义在根节点。可以在任何标签中定义，但只有定义了后才能使用。命名空间还可以嵌套或者被重定义。但这样会增加复杂性。一般用的比较少。一个xml文件中可以拥有多个命名空间。使用命名空间前缀可以轻松处理它们。如：
```
<x:transform version=”1.0” xmlns:x=http://www.w3.org/1999/XSL/Transformxmlns:d=”urn:dm:student”>
<x:template match=”student”/><d:template match=”name”/></x:transform>
```

### 默认的命名空间 ###
为元素定义默认的命名空间可以让我们省去在所有的子元素中使用前缀的工作
xmlns="namespaceURI"
使用默认命名空间后，如果不加前缀则引用默认命名空间。使用默认命名空间会降低xml结构的清晰度。要慎用。


参考：

* http://m.blog.csdn.net/w938706428/article/details/41448821
* http://www.runoob.com/xml/xml-namespaces.html