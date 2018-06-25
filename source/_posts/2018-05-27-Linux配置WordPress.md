---
title: Linux配置WordPress
tags: Linux应用
abbrlink: 96cee6f3
date: 2018-05-13 23:30:59
---

## 安装LAMP
https://lamp.sh/install.html



## 创建WordPress数据库和一个用户

```
mysql>create database wordpress;
//wordpress是数据库名
mysql>create user 'wordpress'@'localhost' identified by 'password';
//wordpress是数据库名，一般使用localhost，password为密码
mysql>grant all privileges on wordpress.* to 'wordpress'@'localhost';
mysql>FLUSH PRIVILEGES;
```


## 下载WordPress安装包并解压
```
wget http://wordpress.org/latest.tar.gz
tar -xzvf latest.tar.gz
```


## 上传文件
默认的网站根目录： /data/www/default
将解压好的wordpress文件复制到该目录下


## 设置wp-config.php文件
（这一步可以略过，通过下一步的网页设置）
将wordpress目录下的wp-config-sample.php重命名为wp-config.php，并修改以下选项
```
/** The name of the database for WordPress */
define('DB_NAME', 'wordpress');

/** MySQL database username */
define('DB_USER', 'wordpress');

/** MySQL database password */
define('DB_PASSWORD', 'password');

/** MySQL hostname */
define('DB_HOST', 'localhost');

/** Database Charset to use in creating database tables. */
define('DB_CHARSET', 'utf8');
```

## 使用
在浏览器访问：http://ip/wordpress，填写好信息一键安装，安装好后就可以是使用了



参考：
https://codex.wordpress.org/zh-cn:%E5%AE%89%E8%A3%85WordPress