---
title: Docker ps 进阶用法
url: 3552.html
id: 3552
categories:
  - Linux
  - 运维
date: 2017-11-08 21:16:48
tags:
---

转载自易百教程网 出处为Docker文档 `docker ps`命令用于列出容器。 **用法**

    docker ps [OPTIONS]
    

**选项**

名称，简写

默认

描述

`--all, -a`

`false`

显示所有容器(默认显示刚刚运行)

`--filter, -f`

根据提供的条件过滤输出

`--format`

使用Go模板打印容器

`--last, -n`

`-l`

显示最后创建的容器(包括所有状态)

`--latest, -l`

`false`

显示最新创建的容器(包括所有状态)

`--no-trunc`

`false`

不要截断输出

`--quiet, -q`

`false`

只显示数字ID

`--size, -s`

`false`

显示文件大小

例子
--

**防止截断输出** 运行`docker ps --no-trunc`显示2个链接的容器。

    $ docker ps
    CONTAINER ID        IMAGE                        COMMAND                CREATED              STATUS              PORTS               NAMES
    4c01db0b339c        ubuntu:12.04                 bash                   17 seconds ago       Up 16 seconds       3300-3310/tcp       webapp
    d7886598dbe2        crosbymichael/redis:latest   /redis-server --dir    33 minutes ago
    

**如何显示运行和停止容器** `docker ps`命令默认情况下仅显示运行容器。 要查看所有容器，请使用`-a`(或`--all`)标志：

    $ docker ps -a

**过滤** 过滤标志(`-f`或`--filter`)格式是`key = value`对。如果有多个过滤器，则传递多个标志(例如`--filter “foo = bar” --filter “bif = baz”`) **标签**过滤器基于单独存在标签或标签和值来匹配容器。 以下过滤器与`color`标签的容器匹配，而不管其值。

    $ docker ps --filter "label=color"
    CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
    673394ef1d4c        busybox             "top"               47 seconds ago      Up 45 seconds                           nostalgic_shockley
    d85756f57265        busybox             "top"               52 seconds ago      Up 51 seconds                           high_albattani
    

以下过滤器与具有`blue`值的`color`标签的容器相匹配。

    $ docker ps --filter "label=color=blue"
    CONTAINER ID        IMAGE               COMMAND             CREATED              STATUS              PORTS               NAMES
    d85756f57265        busybox             "top"               About a minute ago   Up About a minute                       high_albattani
    

**名称**过滤器匹配容器名称的全部或部分。 以下过滤器与包含`nostalgic_stallman`字符串的名称匹配所有容器。

    $ docker ps --filter "name=nostalgic_stallman"
    CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
    9b6247364a03        busybox             "top"               2 minutes ago       Up 2 minutes                            nostalgic_stallman
    

也可以筛选名称中的子字符串，如下所示：

    $ docker ps --filter "name=nostalgic"
    CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
    715ebfcee040        busybox             "top"               3 seconds ago       Up 1 second                             i_am_nostalgic
    9b6247364a03        busybox             "top"               7 minutes ago       Up 7 minutes                            nostalgic_stallman
    673394ef1d4c        busybox             "top"               38 minutes ago      Up 38 minutes                           nostalgic_shockley
    

**exited**过滤器通过存在状态代码匹配容器。 例如，要过滤已成功退出的容器：

    $ docker ps -a --filter 'exited=0'
    CONTAINER ID        IMAGE             COMMAND                CREATED             STATUS                   PORTS                      NAMES
    ea09c3c82f6e        registry:latest   /srv/run.sh            2 weeks ago         Exited (0) 2 weeks ago   127.0.0.1:5000->5000/tcp   desperate_leakey
    106ea823fe4e        fedora:latest     /bin/sh -c 'bash -l'   2 weeks ago         Exited (0) 2 weeks ago                              determined_albattani
    48ee228c9464        fedora:20         bash                   2 weeks ago         Exited (0) 2 weeks ago                              tender_torvalds
    

可以使用过滤器来查找退出状态为`137`的容器，意思是`SIGKILL(9)`将其杀死。

    $ docker ps -a --filter 'exited=137'
    CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS                       PORTS               NAMES
    b3e1c0ed5bfe        ubuntu:latest       "sleep 1000"           12 seconds ago      Exited (137) 5 seconds ago                       grave_kowalevski
    a2eb5558d669        redis:latest        "/entrypoint.sh redi   2 hours ago         Exited (137) 2 hours ago                         sharp_lalande
    

**状态**过滤器按状态匹配容器。可以使用创建，重新启动，运行，删除，暂停，退出和死机进行过滤。例如，要过滤运行容器：

    $ docker ps --filter status=running
    CONTAINER ID        IMAGE                  COMMAND             CREATED             STATUS              PORTS               NAMES
    715ebfcee040        busybox                "top"               16 minutes ago      Up 16 minutes                           i_am_nostalgic
    d5c976d3c462        busybox                "top"               23 minutes ago      Up 23 minutes                           top
    9b6247364a03        busybox                "top"               24 minutes ago      Up 24 minutes                           nostalgic_stallman
    

过滤暂停的容器：

    $ docker ps --filter status=paused
    CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
    673394ef1d4c        busybox             "top"               About an hour ago   Up About an hour (Paused)                       nostalgic_shockley