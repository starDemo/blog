---
title: Nginx HTTPS访问错误解决方案
tags:
  - 运维
  - Linux
  - ngin x
categories: []
toc: false
author: star liu
date: 2018-11-05 23:04:00
---

谷歌报错 ERR_SSL_PROTOCOL_ERROR

火狐报错 SSL 接收到一个超出最大准许长度的记录[ssl_error_rx_record_too_long]
<!-- more -->

# 解决方案
查询解决方案得到如下方案

修改conf文件的server区块
```
listen 443; 
```
为
```
listen 443 default ssl;
```
保存，重启Nginx。完美解决！

#  纠正重新研究发现问题根源

default参数允许所有nginx未指定的域名使用default配置文件，上述配置文件定义访问https的未定向域名到default.conf配置文件下，然后配置文件内并没有导入ssl证书，所以出现报错，重新调整二级域名绑定443不加default，然后default配置文件443 加default ssl，并导入证书，配置通过可用。