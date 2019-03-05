title: 服务器SSH加固
author: star liu
tags:
  - Linux
  - 运维
  - 安全
categories:
  - Linux
  - 运维
date: 2019-03-05 12:14:00
---
多次失败登录即封掉IP，防止暴力破解
<!--more-->
[参考Blog](https://www.cnblogs.com/panblack/p/secure_ssh_auto_block.html)
## 一、系统：
Centos7 64位
```
rpm -qa | grep tcp
yum  –y  install  tcp_wrappers  
```
## 二、方法：
读取/var/log/secure，查找关键字 Failed，例如：
```
Jan 21 22:14:36 gitlab sshd[1631]: Failed password for invalid user admin from 193.201.224.199 port 34257 ssh2
Jan 21 22:14:39 gitlab sshd[1631]: Failed password for invalid user admin from 193.201.224.199 port 34257 ssh2
Jan 21 22:14:40 gitlab sshd[1641]: Failed password for invalid user deploy5 from 139.59.93.89 port 36812 ssh2
Jan 21 22:14:46 gitlab sshd[1664]: Failed password for invalid user router from 193.201.224.199 port 4674 ssh2
Jan 21 22:14:52 gitlab sshd[1675]: Failed password for invalid user root from 193.201.224.199 port 11127 ssh2
Jan 21 22:14:59 gitlab sshd[1692]: Failed password for invalid user admin from 193.201.224.199 port 24144 ssh2
Jan 21 22:15:01 gitlab sshd[1692]: Failed password for invalid user admin from 193.201.224.199 port 24144 ssh2
```
从这些行中提取IP地址，如果次数达到10次(脚本中判断次数字符长度是否大于1)则将该IP写到 /etc/hosts.deny中。
## 三.步骤：
1. 1、先把始终允许的IP填入 /etc/hosts.allow ，==这很重要==！比如：
```
sshd:19.16.18.1:allow
sshd:19.16.18.2:allow
```
2. 脚本 /usr/local/bin/secure_ssh.sh
```
#! /bin/bash
cat /var/log/secure|awk '/Failed/{print $(NF-3)}'|sort|uniq -c|awk '{print $2"="$1;}' > /usr/local/bin/black.list
for i in `cat  /usr/local/bin/black.list`
do
  IP=`echo $i |awk -F= '{print $1}'`
  NUM=`echo $i|awk -F= '{print $2}'`
  if [ ${#NUM} -gt 1 ]; then
    grep $IP /etc/hosts.deny > /dev/null
    if [ $? -gt 0 ];then
      echo "sshd:$IP:deny" >> /etc/hosts.deny
    fi
  fi
done
```
3. 将secure_ssh.sh脚本放入cron计划任务，每1分钟执行一次。
```
# crontab -e
*/1 * * * *  sh /usr/local/bin/secure_ssh.sh
```
## 四、测试：