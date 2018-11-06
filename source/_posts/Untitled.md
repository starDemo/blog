title: kubelet常用的命令以及方法
author: star liu
tags:
  - k8s
  - 运维
categories: []
date: 2018-11-02 08:50:00
---
最近在学习k8s的使用，有一些技巧性的操作放在这里
<!--more-->

# K8s学习使用中的笔记

#### kubelet增加自动补全功能
```
source <(kubectl completion bash)
echo "source <(kubectl completion bash)" >> ~/.bashrc
```

#### kubelet常用的命令以及方法
```
kubectl [command] [TYPE] [NAME] [flags]
```
1. 显示Pod的更多信息
```
kubectl get pod <pod-name> -o wide
```
以yaml格式显示Pod的详细信息
```
kubectl get pod <pod-name> -o yaml
```


#### k8s的一些小坑
在scale pod的过程中从describe 命令的信息中发现deployment的容器不会部署到主节点，查询官网文档发现如下内容：

*By default, your cluster will not schedule pods on the master for security reasons.*
 
 基本意思就是出于安全原因默认仅用了Master节点的Pod部署，所以需要一个命令

```
kubectl taint nodes --all node-role.kubernetes.io/master-
```
执行完后显示
```
node "test-01" untainted
taint "node-role.kubernetes.io/master:" not found
taint "node-role.kubernetes.io/master:" not found
```
[官方文档到这里](https://kubernetes.io/docs/setup/independent/create-cluster-kubeadm/)