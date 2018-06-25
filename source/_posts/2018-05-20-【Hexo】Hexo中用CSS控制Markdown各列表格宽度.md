---
title: 【Hexo】Hexo中用CSS控制Markdown各列表格宽度
tags: hexo
abbrlink: 25ce1142
date: 2018-05-12 09:45:32
---



```
<style>
table th:nth-of-type(1){
width: 10%;
}
table th:nth-of-type(2){
width: 20%;
}
table th:nth-of-type(3){
width: 30%;
}
table th:nth-of-type(4){
width: 40%;
}
</style>

| 一 | 二 | 三 | 四 |
| :-: | :-: | :-: | :-: |
| 1 | 2 | 3 | 4 |

```
<style>
table th:nth-of-type(1){
width: 10%;
}
table th:nth-of-type(2){
width: 20%
;
}
table th:nth-of-type(3){
width: 30%;
}
table th:nth-of-type(4){
width: 40%;
}
</style>


| 一 | 二 | 三 | 四 |
| :-: | :-: | :-: | :-: |
| 1 | 2 | 3 | 4 |


参考：
* https://www.jixian.io/2017/10/11/Hexo%E4%B8%AD%E7%94%A8CSS%E6%8E%A7%E5%88%B6Markdown%E5%90%84%E5%88%97%E8%A1%A8%E6%A0%BC%E5%AE%BD%E5%BA%A6/