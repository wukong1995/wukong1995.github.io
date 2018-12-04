---
date: 2017-03-29 18:22:28
title: 微信公众号搭建
tags: [微信公众号, PHP]
categories: [PHP]
---
使用apache+php5.4n
1、打开apache配置文件httpd.conf
      搜索httpd-vhosts.conf，打开这一行的注释
2、打开apache所在目录下的，conf/extra/httpd-vhost.conf
  增加以下代码，保存
```
<VirtualHost *:80>  
 DocumentRoot "D:\WWW\car-xxxx\car"  
 ServerName www.car-zones.com  
</VirtualHost>  
<Directory "D:\WWW\car-xxxx\car">  
    Options Indexes FollowSymLinks Includes ExecCGI  
    AllowOverride All  
    Order allow,deny  
    Allow from all  
</Directory>  
```
3、重启apache
4、打开c:/windows/system32/drivers/etc/hots
最后一行加入：www.car-xxxx.com
5、访问 www.car-xxxx.com
6、完成配置

第一次学习，记录一下

