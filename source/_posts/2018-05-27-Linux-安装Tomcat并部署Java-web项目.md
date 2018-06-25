---
title: Linux 安装Tomcat并部署Java web项目
tags: Linux应用
abbrlink: 367a9328
date: 2018-05-07 22:10:33
---



## 在Linux上安装JDK
略


## 在Linux上安装FTP
略


## 在Linux上安装Tomcat
下载Tomcat，利用FTP上传到服务器上
```
#解压，位置随意
tar -zxvf apache-tomcat-8.0.29.tar.gz

#防火墙里面开放8080端口
vim /etc/sysconfig/iptables
#重启防火墙
service iptables restart

#启动Tomcat
/usr/local/tomcat/bin/startup.sh

#tomcat自启动 
vim /etc/rc.d/rc.local
/usr/local/tomcat/bin/startup.sh  

```

![](http://ow3dy62zt.bkt.clouddn.com/IMG31.png)

## Idea打包Java web项目

点击Idea左下角，在右侧出现maven project选项，单击maven project选项，出现菜单，选择其中的Lifecycle菜单项，展开，执行里面的package命令即可。 
在项目文件夹/target/下可以找到 \*.war 文件。


## 将war文件部署到tomcat上

1. 将war文件拷贝到tomcat安装目录的`$TOMCAT_HOME/webapps`文件夹下。
2. 修改`$TOMCAT_HOME/conf/server.xml`，在Host配置段中添加类似于如下内容：
```
<Context path="/" docBase="appname.war" debug="0" privileged="true" reloadable="true"/> 
```
docBase=”appname.war”中的appname.war 替换成自己的war的名字。
3. 重启tomcat，然后访问http://localhost:8080/appname 即可。








参考：
* https://blog.csdn.net/to_baidu/article/details/52823402
* https://blog.csdn.net/yums467/article/details/51660683