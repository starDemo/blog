title: mac osx 10.14网卡配置错误
author: star liu
tags:
  - hackintosh
categories: []
date: 2019-01-14 16:36:00
---
RT Hackintosh是一个好东西，但是苹果这bug不修就很不爽了 
<!--more-->

## 症状
无效的服务器地址 BasicIPv6ValidationError错误解决方法

## 解决方案：

思路是这样的：先关闭IPv6，然后设置IPv4，再重新开启IPv6。

update 2018.08.08  我发现其实可以直接用命令行修改IPv4，不用管IPv6，如果它没报错的话



### 1. 关闭 IPv6

显然 ”高级“ > "TCP/IP" 下 IPv6 没有提供关闭选项，所以需要用终端命令 

终端输入：`networksetup -setv6off Ethernet`

### 2. 设置IPv4地址

终端输入：`networksetup -setmanual Ethernet 192.168.31.2 255.255.255.0 192.168.1.1`

对应IP地址、子网掩码、路由器

设置完成后，可以看到，以太网显示状态是：已连接