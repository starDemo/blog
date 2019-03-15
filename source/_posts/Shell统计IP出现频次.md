title: Shell统计IP出现频次
author: star liu
copyright: false
tags: []
categories:
  - Linux
  - 运维
date: 2019-03-15 14:46:00
---
统计文件或者系统内IP出现频率
<!--more-->
## 统计文件里的IP
### 准备文件
首先准备文件demo.txt，内容如下：
```
1 192.168.41.20 
2 192.168.41.21 
3 192.168.41.22 
4 192.168.41.23 
5 192.168.41.24 
6 192.168.41.25
```
### 统计
统计出现次数最多的ip次数：
```
cat demo.txt | awk '{print $2}' | sort | uniq -c | sort -n -r | head -n 1
```
注：
```
awk '{ print $2}'：取数据的第2域（第2列），第一列是标号（1，2，3...）,第二列是ip地址

sort：对IP部分进行排序。

uniq -c：打印每一重复行出现的次数。（并去掉重复行）

sort -n -r：按照重复行出现的次序倒序排列。

head -n 1：取排在第一位的ip地址
```
## 统计`netstat -ntu`命令的结果中出现次数最多的ip地址：

执行命令 `netstat -ntu`，显示结果如下：
```
Active Internet connections (w/o servers) 
Proto Recv-Q Send-Q Local Address Foreign Address State 
tcp 0 0 127.0.0.1:8152 127.0.0.1:4193 TIME_WAIT 
tcp 0 0 127.0.0.1:8152 127.0.0.1:4192 TIME_WAIT 
tcp 0 0 127.0.0.1:8152 127.0.0.1:4196 TIME_WAIT 
tcp 0 0 127.0.0.1:8152 127.0.0.1:4199 TIME_WAIT 
tcp 0 0 127.0.0.1:8152 127.0.0.1:4201 TIME_WAIT 
tcp 0 0 127.0.0.1:8152 127.0.0.1:4204 TIME_WAIT 
tcp 0 0 127.0.0.1:8152 127.0.0.1:4207 TIME_WAIT 
tcp 0 0 127.0.0.1:8152 127.0.0.1:4210 TIME_WAIT 
tcp 0 0 192.168.32.62:41682 192.168.47.27:5431 TIME_WAIT 
tcp 0 0 192.168.32.62:41685 192.168.47.27:5431 TIME_WAIT
```
使用脚本命令进行统计：
```
netstat -ntu | tail -n +3|awk '{ print $5}' | cut -d : -f 1 | sort | uniq -c| sort -n -r | head -n 5
```
统计结果：
```
8 127.0.0.1
2 192.168.47.27
```
注：
```
tail -n +3 :去掉上面用红色标明的两行。

awk '{ print $5}'：取数据的第5域（第5列）

cut -d : -f 1 ：取蓝色部分前面的IP部分。

sort：对IP部分进行排序。

uniq -c：打印每一重复行出现的次数。（并去掉重复行）

sort -n -r：按照重复行出现的次序倒序排列。

head -n 5：取排在前5位的IP 
```
--------------------- 
作者：Hello_刘 
来源：CSDN 
原文：https://blog.csdn.net/xiamoyanyulrq/article/details/81570652
