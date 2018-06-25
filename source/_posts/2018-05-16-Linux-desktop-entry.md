---
title: Linux desktop entriy
tags: Linux
abbrlink: ff74c6a5
date: 2018-05-07 11:17:37
---

 

在 Windows 平台上，用户可以通过点击位于桌面或菜单上的快捷方式轻松打开目标应用程序。现代 Linux 桌面系统的应用程序是通过`*.desktop`文件管理的。

一个应用程序对应一个`.desktop`文件，根据是用户自己可见还是所有用户可见的不同而放在 `~/.local/share/applications `或者 `/usr/share/applications/` 目录中。





可以自己添加.desktop文件来增加应用程序到launcher里，`*.desktop`文件格式如下：

```
[Desktop Entry]
Encoding=UTF-8
Name=火狐浏览器
Name[en]=Firefox
Name[en_US]=Firefox
Comment=Firefox for jason
Exec=/opt/firefox/firefox
Icon=/opt/firefox/browser/icons/mozicon128.png
Terminal=false
Type=Application
Categories=Application;Network;
```



使用流程：
1. 创建文件，以.desktop为后缀。
2. 编写内容，修改权限
3. 测试是否能双击启动程序
4. 移动到/usr/share/applications/目录下


文件中不能有多余的全角或半角空格，否则会出错








参考：

* https://linux.cn/article-9199-1.html
* https://wiki.archlinux.org/index.php/Desktop_entries_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)
* https://www.ibm.com/developerworks/cn/linux/l-cn-dtef/index.html
* https://blog.csdn.net/lwjdgl/article/details/49204659



