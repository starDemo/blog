---
title: 玩转git之webhook应用初探(转载)
url: 3372.html
id: 3372
categories:
  - 杂七杂八技术
  - 杂侃
date: 2016-04-15 10:53:18
tags:
---

![git](http://ued.ctrip.com/blog/wp-content/uploads/2014/07/git.png)  具体步骤： **服务器端：** 1\. 服务器端 生成 apache 的 deploy key

> sudo -u apache ssh-keygen -t rsa -C “jianl@example.com”

2.给apache 操作目录的权限

> 第一种方法 直接给 目录 777 权限 简单粗暴 第二种方法 建立用户组  把 ftp用户 和 apache 添加到该组别里面 ， 给予该组权限
> 
>     groupadd gitwriters
>     adduser [yourusername] gitwriters
> 
>     adduser apache gitwriters
> 
>     chgrp -R gitwriters /path/to/your/repo
>     
> 
>     chmod -R g+rw /path/to/your/repo

 3.在需要自动同步的仓库打开hook

>     cd /项目/.git/
>     cp hooks/post-receive.sample hooks/post-receive
>     vim hooks/post-receive
>     #加入下面代码
>     GIT_WORK_TREE=/home/www git checkout -f
>     

 4.加入接收 webhook的 脚本，  在项目 创建 update.php

>     $www_folder = "/2T/ftp/utools/uilib" ;
>     //git仓库地址
>     $git_repo = "git@git.dev/.......abc.git" ;
>     //执行指令
>     echo shell_exec(" cd $www_folder && git pull $git_repo 2>&1 ");

**gitlab 端的设置：**

> 找到项目的设置 ，
> deploy key项   添加  直接 apache用户 生成的 ssh key
> webhook 项 添加 网站地址/update.php （正常能够访问的链接）  ， 勾选  **Push events** 保存

完全以上配置 。服务器端自动同步代码的功能就能够实现了。 转载来自:[**携程设计委员会**](http://ued.ctrip.com/blog) 原作者：l, jian