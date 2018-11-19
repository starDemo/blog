title: vim使用过程中的一些坑
author: star liu
tags:
  - 运维
  - Linux
categories: []
date: 2018-11-19 17:54:00
---
主要记录了使用vim编辑器过程中踩的一些坑以及一些技巧～
<!--more-->
# VIM的一些使用备忘

## vim粘贴代码时自动缩进造成的错误
```
:set paste
```
退出粘贴模式
```
set nopaste
```
