title: golang 写入文件的一些备忘
url: 3617.html
id: 3617
categories:
  - Codeing
date: 2018-09-11 17:47:47
tags:
---
go学习过程中的笔记
<!--more-->
操作函数 os.OpenFile

os.O\_WRONLY | os.O\_CREATE | O_EXCL          

【如果已经存在，则失败】

os.O\_WRONLY | os.O\_CREATE                        

【如果已经存在，会覆盖写，不会清空原来的文件，而是从头直接覆盖写】

os.O\_WRONLY | os.O\_CREATE | os.O_APPEND 

【如果已经存在，则在尾部添加写】