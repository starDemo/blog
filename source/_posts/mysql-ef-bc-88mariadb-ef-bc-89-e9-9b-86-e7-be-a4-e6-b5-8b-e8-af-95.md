---
title: Mysql（mariadb）集群测试
url: 3340.html
id: 3340
categories:
  - 运维
date: 2016-03-01 23:16:02
tags:
---

测试环境：Vmware （Centos 7 x3）

1.修改Centos的软件源，添加MariaDB的软件源，并安装MariaDB-Galera-server

\[root@www ~\]# vi /etc/yum.repos.d/mariadb.repo

\# MariaDB 10.0 CentOS repository list
\# http://mariadb.org/mariadb/repositories/
\[mariadb\]
name = MariaDB
baseurl = http://yum.mariadb.org/10.0/centos7-amd64
gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
gpgcheck=1
enabled=0

\[root@www ~\]# yum --enablerepo=mariadb -y install MariaDB-Galera-server

2.修改主控节点

vim /etc/my.cnf.d/server.cnf

\[galera\]
\# Mandatory settings
wsrep\_provider=/usr/lib64/galera/libgalera\_smm.so
\# specify all nodes in cluster
wsrep\_cluster\_address="gcomm://10.0.0.31,10.0.0.51"
\# uncomment all
binlog_format=row
default\_storage\_engine=InnoDB
innodb\_autoinc\_lock_mode=2
bind-address=0.0.0.0
\# add follows
\# cluster name
wsrep\_cluster\_name="MariaDB_Cluster"
\# own IP address
wsrep\_node\_address="10.0.0.31"
\# replication provider
wsrep\_sst\_method=rsync

启动数据库集群主控端

 \[root@www ~\]# /etc/rc.d/init.d/mysql bootstrap
Bootstrapping the cluster.. Starting MySQL.. SUCCESS!

初始化数据库(只需要在主控端进行）

\[root@www ~\]# mysql\_secure\_installation

3.配置数据节点(依次配置每个节点）

vim /etc/my.cnf.d/server.cnf

\[galera\]
\# Mandatory settings
wsrep\_provider=/usr/lib64/galera/libgalera\_smm.so
\# specify all nodes in cluster
wsrep\_cluster\_address="gcomm://10.0.0.31,10.0.0.51"
\# uncomment all
binlog_format=row
default\_storage\_engine=InnoDB
innodb\_autoinc\_lock_mode=2
bind-address=0.0.0.0
\# add follows
\# cluster name
wsrep\_cluster\_name="MariaDB_Cluster"
\# own IP address
wsrep\_node\_address="10.0.0.51"
\# replication provider
wsrep\_sst\_method=rsync

启动节点数据库服务

\[root@node01 ~\]# systemctl start mysql

4.集群搭建基本完成 下面进行测试

\[root@node01 ~\]# mysql -u root -p
Enter password:
Welcome to the MariaDB monitor.  Commands end with ; or \\g.
Your MariaDB connection id is 6
Server version: 10.0.20-MariaDB-wsrep MariaDB Server, wsrep_25.10.r4144
Copyright (c) 2000, 2015, Oracle, MariaDB Corporation Ab and others.
Type 'help;' or '\\h' for help. Type '\\c' to clear the current input statement.
MariaDB \[(none)\]> show status like 'wsrep_%';

数据库回显：

MariaDB \[(none)\]> show status like 'wsrep_%';
+------------------------------+--------------------------------------+
| Variable_name                | Value                                |
+------------------------------+--------------------------------------+
| wsrep\_local\_state_uuid       | 20329169-3414-11e5-9285-cab5ed757f81 |
| wsrep\_protocol\_version       | 7                                    |
| wsrep\_last\_committed         | 3                                    |
| wsrep_replicated             | 0                                    |
| wsrep\_replicated\_bytes       | 0                                    |
| wsrep\_repl\_keys              | 0                                    |
| wsrep\_repl\_keys_bytes        | 0                                    |
| wsrep\_repl\_data_bytes        | 0                                    |
| wsrep\_repl\_other_bytes       | 0                                    |
| wsrep_received               | 3                                    |
| wsrep\_received\_bytes         | 237                                  |
| wsrep\_local\_commits          | 0                                    |
| wsrep\_local\_cert_failures    | 0                                    |
| wsrep\_local\_replays          | 0                                    |
| wsrep\_local\_send_queue       | 0                                    |
| wsrep\_local\_send\_queue\_max   | 2                                    |
| wsrep\_local\_send\_queue\_min   | 0                                    |
| wsrep\_local\_send\_queue\_avg   | 0.333333                             |
| wsrep\_local\_recv_queue       | 0                                    |
| wsrep\_local\_recv\_queue\_max   | 1                                    |
| wsrep\_local\_recv\_queue\_min   | 0                                    |
| wsrep\_local\_recv\_queue\_avg   | 0.000000                             |
| wsrep\_local\_cached_downto    | 18446744073709551615                 |
| wsrep\_flow\_control\_paused\_ns | 0                                    |
| wsrep\_flow\_control_paused    | 0.000000                             |
| wsrep\_flow\_control_sent      | 0                                    |
| wsrep\_flow\_control_recv      | 0                                    |
| wsrep\_cert\_deps_distance     | 0.000000                             |
| wsrep\_apply\_oooe             | 0.000000                             |
| wsrep\_apply\_oool             | 0.000000                             |
| wsrep\_apply\_window           | 0.000000                             |
| wsrep\_commit\_oooe            | 0.000000                             |
| wsrep\_commit\_oool            | 0.000000                             |
| wsrep\_commit\_window          | 0.000000                             |
| wsrep\_local\_state            | 4                                    |
| wsrep\_local\_state_comment    | Synced                               |
| wsrep\_cert\_index_size        | 0                                    |
| wsrep\_causal\_reads           | 0                                    |
| wsrep\_cert\_interval          | 0.000000                             |
| wsrep\_incoming\_addresses     | 10.0.0.31:3306,10.0.0.51:3306        |
| wsrep\_evs\_delayed            |                                      |
| wsrep\_evs\_evict_list         |                                      |
| wsrep\_evs\_repl_latency       | 0/0/0/0/0                            |
| wsrep\_evs\_state              | OPERATIONAL                          |
| wsrep\_gcomm\_uuid             | 69d7b95d-3415-11e5-9666-7be9d4b6159d |
| wsrep\_cluster\_conf_id        | 2                                    |
| wsrep\_cluster\_size           | 2                                    |
| wsrep\_cluster\_state_uuid     | 20329169-3414-11e5-9285-cab5ed757f81 |
| wsrep\_cluster\_status         | Primary                              |
| wsrep_connected              | ON                                   |
| wsrep\_local\_bf_aborts        | 0                                    |
| wsrep\_local\_index            | 1                                    |
| wsrep\_provider\_name          | Galera                               |
| wsrep\_provider\_vendor        | Codership Oy <info@codership.com>    |
| wsrep\_provider\_version       | 25.3.9(r3387)                        |
| wsrep_ready                  | ON                                   |
| wsrep\_thread\_count           | 2                                    |
+------------------------------+--------------------------------------+
57 rows in set (0.00 sec)

wsrep\_local\_state\_comment | Synced 表示数据库同步正常 可以对其中的一个节点数据库进行增删改 然后经由其他节点进行查看 同步正常 写在最后： 本次Blog是经过测试 参考资料 http://www.server-world.info/en/note?os=CentOS\_7&p=mariadb&f=4 https://v2ex.com/t/202975 http://galeracluster.com/documentation-webpages/?id=mysql\_galera\_configuration PS：v2ex教材待测试 同时尝试将集群做到Docker内作为应用打包方案 以及Debian的尝试部署