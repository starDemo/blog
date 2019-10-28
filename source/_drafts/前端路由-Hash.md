title: 前端路由-Hash
author: star liu
tags:
  - 前端
categories:
  - 前端
date: 2019-10-28 09:29:00
---
最近学习goframe框架时涉及到一个前端框架整合，以静态文件模式加载，前后端分离模式运行的，结果遇到了路由问题，其中涉及到前端路由中的hash使用。
<!--more-->
## url中的`hash(#)`
### 1. `hash(#)`的含义
#代表网页中的一个位置，其右边的字符，就是该位置的标识符。比如
```
http://loaclhost/index.html#user
```
### 2.HTTP请求不包含#
### 3.`#`后面的字符
### 4.改变`#`不触发网页重载
### 5.改变`#`会改变浏览器的访问历史
### 6.`window.location.hash` 读取#值
### 7.`onhashchange`事件
