---
title: 【Hexo】使用Hexo搭建github博客
tags: hexo
abbrlink: a20da6f3
date: 2016-10-09 19:23:21
---

1.创建github帐号，新建一个以`用户名.github.io`命名的仓库

2.安装git，创建ssh
```
ssh-keygen -t rsa -f ~/.ssh/id_rsa_github
#用于创建ssh，会有公钥和私钥，将公钥中的内容拷贝到github帐号 Settings > SSH and GPG keys 中

ssh -T git@github.com
#用于测试是否成功
```
输入yes，其他使用默认即可，配置git
```
git config --global user.name "cnfeat"//用户名
git config --global user.email  "cnfeat@gmail.com"//邮箱
```
3.下载并安装hexo

4.新建一个目录用于放置本地博客
```
mkdir hexo
cd hexo
npm install hexo-cli -g 
hexo init blog
cd blog
npm install
hexo g
hexo s   #可以在本地http://localhost:4000/查看
```
5.修改blog目录下的_config.yml文件中的deploy：
```
deploy:
type: git
repository: git@github.com:xxxxxx/xxxxxxx.github.io.git
branch: master
```
6.安装hexo中关于git的组件，上传部署（需要cd到hexo\node_modules\hexo\Hexo目录）
```		
npm install hexo-deployer-git --save

hexo clean
hexo g
hexo d
```
如果出现问题请参考[使用hexo d无法上传问题解决办法](http://fengrenxiaoli.github.io/2016/10/09/%E4%BD%BF%E7%94%A8hexo-d%E6%97%A0%E6%B3%95%E4%B8%8A%E4%BC%A0%E9%97%AE%E9%A2%98%E8%A7%A3%E5%86%B3%E5%8A%9E%E6%B3%95/)然后再使用以上代码

参考：

* http://kiya.space/2015/11/10/use-Github-Pages-Hexo-duoshuo-to-set-up-a-blog-basic-steps/
* http://sunwhut.com/2015/10/30/buildBlog/
* https://my.oschina.net/ryaneLee/blog/638440
* http://www.jianshu.com/p/ab21abc31153
* https://www.haomwei.com/technology/maupassant-hexo.html

