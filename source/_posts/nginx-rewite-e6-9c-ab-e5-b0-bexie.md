title: Nginx Rewite 末尾修饰
url: 3621.html
id: 3621
categories:
  - Linux
  - 日常
  - 运维
date: 2018-09-17 16:23:24
tags:
---
今天SEO部门丢来需求，统一业务平台的链接尾缀 嗯 统一删除斜杠  爬文解决
<!--more-->

### 1\. 在URL结尾添加斜杠

在虚拟主机中这么添加一条改写规则：

1

rewrite ^(.*\[^/\])$ $1/ permanent;

例如：

    server {
        listen 80;
        server_name bbs.ttlsa.com;
        rewrite ^(.*[^/])$ $1/ permanent;
    }

### 2\. 删除URL结尾的斜杠

在虚拟主机中这么添加一条改写规则：

1

rewrite ^/(.*)/$ /$1 permanent;

例如：

12345

server {    listen 80;    server_name bbs.ttlsa.com;    rewrite ^/(.*)/$ /$1 permanent;}

不过建议删除URL结尾的斜杠，会混乱搜索引擎的。

引申[nginx](http://www.ttlsa.com/nginx/)重写规则参见《[Nginx重写规则指南](http://www.ttlsa.com/nginx/nginx-rewriting-rules-guide/)》。