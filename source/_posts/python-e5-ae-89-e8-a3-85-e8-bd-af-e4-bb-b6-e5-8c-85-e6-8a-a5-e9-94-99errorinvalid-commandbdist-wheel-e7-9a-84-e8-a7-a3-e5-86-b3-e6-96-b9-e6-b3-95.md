title: 'python安装软件包报错error:invalid command''bdist_wheel''的解决方法'
url: 3601.html
id: 3601
categories:
  - Linux
  - Python
  - 日常
  - 运维
date: 2018-05-18 02:26:55
tags:
---



# 原因：
pip和setuptools的版本较低。 
<!--more-->
# 解决方案：
升级pip和setuptools。 

**一、升级pip** 升级pip的方法： 在cmd里执行：pip install --upgrade pip（该语句是pip升级自己） 

**二、升级setuptools** 升级setuptools的方法： 方法①：在cmd里执行该语句：pip install setuptools --upgrade 方法②：到官网去下载最新的setupools安装包 https://pypi.python.org/pypi/setuptools     点击downloads，下载setuptools-28.7.1.tar.gz（或其他格式的，下载时间不同版本号可能会不同） 将该文件下载到本地后进行解压，在cmd中进入到setup.py文件所在的目录，执行python setup.py install