---
title: Shell使用技巧合辑
tags:
  - 运维
originContent: ''
categories:
  - Linux
  - 运维
  - Shell
toc: false
date: 2019-04-22 13:49:06
---

记录一些使用Shell的技巧
<!--more-->
## Shell中的文字处理
- JSON
```bash
# jq是一个Shell中的常用json对象化工具
# 安装
yum install -y jq
# 使用Demo Data见后文 如下命令获取
kubectl get secrets -n test test -ojson
########Data##########################
kubectl get secrets -n test test -ojson|jq '.data.passwd'
"aGVsbG8="
```
Data
```json
{
    "apiVersion": "v1",
    "data": {
        "passwd": "aGVsbG8="
    },
    "kind": "Secret",
    "metadata": {
        "annotations": {
            "field.cattle.io/creatorId": "u-9hs5v",
            "field.cattle.io/projectId": "c-6hbc7:p-l8wp7",
            "lifecycle.cattle.io/create.secretsController_c-6hbc7": "true",
            "secret.user.cattle.io/secret": "true"
        },
        "creationTimestamp": "2019-04-22T05:29:28Z",
        "name": "test",
        "namespace": "test",
        "resourceVersion": "472498492",
        "selfLink": "/api/v1/namespaces/cms3/secrets/test",
        "uid": "9985b4ea-64bf-11e9-9da9-06976d75b572"
    },
    "type": "Opaque"
}

```


- String 字符串处理去除引号
```bash
kubectl get secrets -n test test -ojson|jq '.data.passwd'
"aGVsbG8="
kubectl get secrets -n cms3 test -ojson|jq '.data.passwd'|sed 's/\"//g'
aGVsbG8=
```

