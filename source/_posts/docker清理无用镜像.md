title: docker清理无用镜像
author: star liu
tags:
  - docker
  - k8s
categories:
  - 运维
date: 2019-09-19 11:21:00
---
docker，kubernetes使用过程中镜像越来越多 清理镜像以缓解磁盘压力
<!--more-->

**为了以防万一（线上环境一定要谨慎谨慎再谨慎** 

- 1、先使用`kubectl get po –namespace `命名空间，查看该命名空间已有的pod
- 2、重新部署pod，在该node节点上产生多余的images镜像
- 3、使用`docker system df`命令，在执行清除镜像之前先查看镜像和容器的数量。
注：类似于Linux上的df命令，用于查看Docker的磁盘使用情况。这条命令可以查看到node节点中镜像和容器的数量
- 4、使用`docker system prune –a`。清除无用的镜像
注：`docker system prune`命令可以用于清理磁盘，删除关闭的容器、无用的数据卷和网络，以及dangling镜像(即无tag的镜像)。`docker system prune -a`命令清理得更加彻底，可以将没有容器使用Docker镜像都删掉。注意，这两个命令会把你暂时关闭的容器，以及暂时没有用到的Docker镜像都删掉了…所以使用之前一定要想清楚吶。
- 5、使用`docker system df`命令，查看镜像和容器的数量；是否成功