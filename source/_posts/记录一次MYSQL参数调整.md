title: 记录一次MYSQL参数调整
author: star liu
tags:
  - Linux
  - 运维
  - 数据库
categories: []
date: 2019-10-12 09:22:00
---
公司运维基础架构迭代过程中，MYSQL由5.6升级至5.7过程中遇到5.7的强规范限制导致的不兼容老系统，修改参数以适应原有系统。

<!--more-->
# MySQL的参数调整
## 问题
因为业务老旧sql原因需要禁用`ONLY_FULL_GROUP_BY`

- 查询MYSQL版本
```shell
mysql> select version();
```
- sql_mode查询
```shell
mysql> select @@GLOBAL.sql_mode;
+-------------------------------------------------------------------------------------------------------------------------------------------+
| @@GLOBAL.sql_mode                                                                                                                         |
+-------------------------------------------------------------------------------------------------------------------------------------------+
| ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION |
+-------------------------------------------------------------------------------------------------------------------------------------------+
1 row in set

mysql> 
```

## 解决办法

重新设置sql_mode，来关闭这个选项。
- 1.临时禁用
在sql终端中执行如下
```shell
mysql> set @@GLOBAL.sql_mode = 'STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION';
mysql> 
```
- 2.永久修改
在mysql配置文件的mysqld节点下增加如下内容
```
sql_mode='STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION'
```

## k8s模式configmap注入
- 1.原理

 永久修改mysqld节点下参数，使用k8s的`volumeMounts`挂入路径`/etc/mysql/mysql.conf.d/mysqld.cnf`
- 2.ConfigMap内容

  ```yaml
  apiVersion: v1
  data:
    mysqld.cnf: |-
      # Copyright (c) 2014, 2016, Oracle and/or its affiliates. All rights reserved.
      #
      # This program is free software; you can redistribute it and/or modify
      # it under the terms of the GNU General Public License as published by
      # the Free Software Foundation; version 2 of the License.
      #
      # This program is distributed in the hope that it will be useful,
      # but WITHOUT ANY WARRANTY; without even the implied warranty of
      # MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
      # GNU General Public License for more details.
      #
      # You should have received a copy of the GNU General Public License
      # along with this program; if not, write to the Free Software
      # Foundation, Inc., 51 Franklin St, Fifth Floor, Boston, MA  02110-1301 USA

      #
      # The MySQL  Server configuration file.
      #
      # For explanations see
      # http://dev.mysql.com/doc/mysql/en/server-system-variables.html

      [mysqld]
      pid-file        = /var/run/mysqld/mysqld.pid
      socket          = /var/run/mysqld/mysqld.sock
      datadir         = /var/lib/mysql
      log-error       = /var/log/mysql/error.log
      # Disabling symbolic-links is recommended to prevent assorted security risks
      symbolic-links=0
      sql_mode='STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION'
  kind: ConfigMap
  metadata:
    name: mysql-config-add
    namespace: cb-common

  ```