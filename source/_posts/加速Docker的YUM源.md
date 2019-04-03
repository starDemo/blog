---
title: 加速Docker的YUM源
tags:
  - Linux
  - 运维
  - docker
originContent: ''
categories:
  - Linux
  - 运维
toc: false
date: 2019-04-03 09:27:02
---

经常Centos部署遇到Docker官方YUM源速度较慢，记录一下换清华加速源操作备查
<!---more-->
Centos操作如下
```shell
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
wget -O /etc/yum.repos.d/docker-ce.repo https://download.docker.com/linux/centos/docker-ce.repo
sudo sed -i 's+download.docker.com+mirrors.tuna.tsinghua.edu.cn/docker-ce+' /etc/yum.repos.d/docker-ce.repo
yum makecache fast
yum install -y docker-ce
```

