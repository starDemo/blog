title: Docker远程调试Golang
author: star liu
tags:
  - docker
  - 杂谈
categories: []
date: 2019-12-11 14:01:00
---
满脑子骚操作之 --  VSCode远程调试--- Docker篇 
<!--more-->

### 配置VSCode连接远程Docker
#### 1. 远程Docker调试,开启api访问
- 修改`systemd`配置`vim /lib/systemd/system/docker.service`
- 在`ExecStart`操作后增加 ` -H tcp://0.0.0.0:2375`  

#### 2. 配置VSCode连接远程Docker
- VSCode安装插件 Docker && 远程调试工具
- 配置VSCode DOCKER_HOST配置 `tcp://ip:2375`

#### 3. 注意事项
- DockerAPI端口注意使用`2375`端口

### 远程调试Docker内的Golang程序
#### 1.备注
- Q: 遇到`dlv`调试程序`could not launch process: fork/exec ./debug: operation not permitted`
 - A: docker run 过程中增加`--security-opt=seccomp:unconfined`参数
 [delve issue 515](https://github.com/go-delve/delve/issues/515) or
 [stackoverflow](https://stackoverflow.com/questions/35827819/fork-exec-debug-operation-not-permitted)
 
- Q: 
 - A: 


### TODO补充