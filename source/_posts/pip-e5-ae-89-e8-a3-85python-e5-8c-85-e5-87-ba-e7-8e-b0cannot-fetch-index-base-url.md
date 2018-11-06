---
title: pip安装python包出现Cannot fetch index base URL
url: 3355.html
id: 3355
categories:
  - Python
date: 2016-03-17 17:59:14
tags:
---

pip install *** 安装python包，出现Cannot fetch index base URL http://pypi.python.org/simple/错误提示或者直接安装不成功。 解决办法： 1.windows下创建/%user%/pip/pop.ini，并添加以下内容。 \[global\] index-url=http://pypi.douban.com/simple/ 2.linux创建文件~/.pip/pip.conf，并添加一下内容。 \[global\] index-url=http://pypi.douban.com/simple/ 3.再次使用pip安装相应的包即可。