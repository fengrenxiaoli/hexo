---
title: Linux 安装Apache
tags: Linux应用
abbrlink: e76285ac
date: 2018-05-07 14:15:33
---



安裝Apache
```
sudo yum install httpd
```

启动Apache，并且设定为开机自动启动
```
sudo systemctl start httpd
sudo systemctl enable httpd
```

允许http服务通过防火墙
CentOS 7的内置防火墙默认设置为阻止网络流量
```
sudo firewall-cmd --add-service=http --permanent
sudo firewall-cmd --add-service=https --permanent
#sudo firewall-cmd --permanent --add-port=80/tcp
#sudo firewall-cmd --permanent --add-port=443/tcp
sudo systemctl restart firewalld
```

重启Apache
```
sudo systemctl restart httpd.service
```





配置文件

`/etc/httpd/conf/httpd.conf`
```
DocumentRoot "/var/www/html/example.com/public_html"

...

<IfModule prefork.c>
    StartServers        5
    MinSpareServers     20
    MaxSpareServers     40
    MaxRequestWorkers   256
    MaxConnectionsPerChild 5500
</IfModule>
```

`/etc/httpd/conf.d/vhost.conf`
```
NameVirtualHost *:80

<VirtualHost *:80>
    ServerAdmin webmaster@example.com
    ServerName example.com
    ServerAlias www.example.com
    DocumentRoot /var/www/html/example.com/public_html/
    ErrorLog /var/www/html/example.com/logs/error.log
    CustomLog /var/www/html/example.com/logs/access.log combined
</VirtualHost>
```
创建上面引用的目录：
```
sudo mkdir -p /var/www/html/example.com/{public_html,logs}
```

检查Apache的状态
```
sudo systemctl status httpd
```

停止Apache
```
sudo systemctl stop httpd
```



参考：

https://www.linode.com/docs/web-servers/apache/install-and-configure-apache-on-centos-7/
https://www.liquidweb.com/kb/how-to-install-apache-on-centos-7/
SELinux：https://www.brilliantcode.net/170/centos-7-install-apache-httpd/
SELinux：https://www.brilliantcode.net/145/centos-7-check-selinux-status-enabled-or-not/










