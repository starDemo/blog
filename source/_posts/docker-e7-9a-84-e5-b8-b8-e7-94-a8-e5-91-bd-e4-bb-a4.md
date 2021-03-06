title: Docker的常用命令
url: 3311.html
id: 3311
categories:
  - 运维
date: 2016-01-22 11:24:35
tags:
---
常用命令的备忘记录
<!--more-->

#### 在进行docker学习过程中通过查询发现如下命令使用频率较高，故作此备忘

1.  查看容器的root用户密码
    
    docker logs <容器名orID> 2>&1 | grep '^User: ' | tail -n1
    
    因为docker容器启动时的root用户的密码是随机分配的。所以，通过这种方式就可以得到redmine容器的root用户的密码了。
2.  查看容器日志
    
    docker logs -f <容器名orID>
    
3.  查看正在运行的容器
    
    docker ps
    
    `docker ps -a`为查看所有的容器，包括已经停止的。
4.  删除所有容器
    
    docker rm $(docker ps -a -q)
    
    删除单个容器`docker rm <容器名orID>`
5.  停止、启动、杀死一个容器
    
    docker stop <容器名orID>
    docker start <容器名orID>
    docker kill <容器名orID>
    
    可以搭配$(docker ps -a -q) 对所有容器操作
    
6.  查看所有镜像
    
    docker images
    
7.  删除所有镜像
    
    docker rmi $(docker images | grep none | awk '{print $3}' | sort -r)
    
8.  运行一个新容器，同时为它命名、端口映射、文件夹映射。以redmine镜像为例
    
    docker run --name redmine -p 9003:80 -p 9023:22 -d -v /var/redmine/files:/redmine/files -v     /var/redmine/mysql:/var/lib/mysql sameersbn/redmine
    
9.  一个容器连接到另一个容器
    
    docker run -i -t --name sonar -d -link mmysql:db   tpires/sonar-server
    
    sonar容器连接到mmysql容器，并将mmysql容器重命名为db。这样，sonar容器就可以使用db的相关的环境变量了。
10.  拉取镜像
    
    docker pull <镜像名:tag>
    
    如`docker pull sameersbn/redmine:latest`
11.  当需要把一台机器上的镜像迁移到另一台机器的时候，需要保存镜像与加载镜像。机器a
    
    docker save busybox-1 > /home/save.tar
    
    使用scp将save.tar拷到机器b上，然后：
    
    docker load < /home/save.tar
    
12.  构建自己的镜像
    
    docker build -t <镜像名> <Dockerfile路径>
    
    如Dockerfile在当前路径：`docker build -t xx/gitlab .`
13.  将容器固化为镜像
    
        docker commit [容器ID]  tag/name