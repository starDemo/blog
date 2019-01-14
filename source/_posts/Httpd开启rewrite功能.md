title: Httpd开启rewrite功能
author: star liu
tags:
  - apache
  - 运维
categories:
  - 运维
  - Linux
date: 2019-01-14 16:39:00
---
帮公司打包业务Docker images时发现rewrire规则默认没有打开，查询解法如下
<!--more-->
## apache开启rewrite重写
### 命令开启
```bash
sudo a2enmod rewrite
sudo /etc/init.d/apache2 restart
```
### 文件修改启动
在终端输入：
```bash
sudo a2enmod rewrite  #开启Rewrite模块（停用模块，使用 a2dismod）

sudo gedit /etc/apache2/sites-available/default 
```
修改下面的地方
```apache
<Directory />

Options FollowSymLinks

AllowOverride None（修改为AllowOverride All）

</Directory>
```
```apache
<Directory "/var/orioner">

Options Indexes FollowSymLinks MultiViews

AllowOverride None（修改为AllowOverride All）

Order allow,deny

allow from all

</Directory>

```
最后
```bash
sudo /etc/init.d/apache2 restart。
```
### 建立htaccess
在网站下面建立.htaccess文件

修改.htaccess文件属性  chmod -R 777 .htaccess

