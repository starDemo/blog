title: 关于如何使用Docker构建PHP的开发环境
url: 16.html
id: 16
categories:
  - 运维
date: 2015-12-30 23:24:46
tags:
---
使用Docker 构建PHP运行开发环境 （碳素云实习期间实践）
<!--more-->
本文作者是Geoffrey，他是一个PHP的Web开发者，喜欢DevOps和Docker。本文主要介绍了如何使用Docker构建PHP的开发环境，文中作者也探讨了构建基于Docker的开发环境应该使用单容器还是多容器，各有什么利弊。推荐PHP开发者阅读。   本文由php100.com dockerone翻译，本博属应用转载。
现在很多开发者都使用Vagrant来管理他们的虚拟机开发环境，Vagrant确实很酷， 不过也有不少缺点（最主要的是它占用太多的资源）。在容器技术、Docker和更多类Docker技术出现后，解决这个问题就变得简单了。

免责声明
----

由于`boot2docker`的工作方式，本文所述的方法在你的环境中可能无法正常运行。如果需要在非Linux环境下共享文件夹到Docker容器，还需要注意更多额外的细节。后续我会写篇文章专门来介绍实际遇到的问题。

怎样才算是好的开发环境
-----------

首先，我们得知道什么才是好的开发环境， 对于我而言，一个好的开发环境需要具备以下几个特点：

1.  可随意使用。我必须可以随意删除和创建新的环境。
2.  快速启动。我想要用它工作时候，它立马就能用。
3.  易于更新。在我们行业中，事物发展变化非常快，必须能让我很容易将我的开发环境更新到新的软件版本。

而Docker都支持以上这些特点，甚至更多。你几乎可以即时销毁和重建容器，而更新环境只需要重建你当前使用的镜像即可。

什么是PHP开发环境
----------

目前Web应用错综复杂，PHP开发环境需要很多的东西，为了保证环境的简单性，需要做各种各样的限制。 我们这次使用Nginx、PHP5-FPM、MySQL来运行Synmfony项目。由于在容器中运行命令行会更复杂，所以这方面的内容我会放到下一篇博客中再说。

Pet 与 Cattle
------------

另一个我们要讨论的重点是：我们要把开发环境部署在多容器还是单容器中。 两种方式各有优点：

*   单容器易于分发、维护。因为它们是独立的，所有的东西都运行在同一个容器中，这点就像是一个虚拟机。但这也意味着，当你要升级其中的某样东西（比如PHP新版本）的时候， 需要重新构建整个容器。
*   多容器可以在添加组件时提供更好的模块化。因为每个容器包含了堆栈的一部分：Web、PHP、MySQL等，这样可以单独扩展每个服务或者添加服务，并且不需要重建所有的东西。

因为我比较懒，加上我需要在我的笔记本上放点别的内容，所以，这里我们只介绍单个容器的方法。

初始化工程
-----

首先要做的是初始化一个新的Symfony工程. 推荐的方法是用`composer`的`create-project`命令。本来可以在工作站上安装composer，但是那样太简单了。这次我们通过Docker来使用它。 我之前发过一篇关于Docker命令的文章：[`make docker commands`](http://geoffrey.io/making-docker-commands.html)（好吧，我说谎了，我本来把它写在这篇文章中了，然后觉得把它独立出来会比较好）。 不管怎么样，你可以读一下。接下来如果还没有`composer`命令的话，你可以创建一个属于自己的`composer` 别名。

$ alias composer="docker run -i -t -v \\$PWD:/srv ubermuda/composer"

现在你可以初始化Symfony工程了：

$ composer create-project symfony/framwork-standard-edition SomeProject

帅呆了！下面来点实在的工作。(省略了博主自娱自乐的一堆balabla....原文:Awesome. Give yourself a high-five， get a cup of coffee or whatever is your liquid drug of choice， and get ready for the real work.)

容器
--

构建一个运行标准Symfony项目且自给自足的容器相当容易，只需要安装好常用的Nginx、PHP5-FPM和MySQL-Server即可，然后把预先准备好的Nginx的虚拟主机配置文件扔进去，再复制一些配置文件进去就完事了。 本容器的源代码在GitHub上的 [ubermuda/docker-symfony](https://github.com/ubermuda/docker-symfony)仓库中可以找到。 Dockerfile 是Docker构建镜像要用到的配置文件，我们来看一下:

FROM debian:wheezy
ENV DEBIAN_FRONTEND noninteractive
RUN apt-get update -y
RUN apt-get install -y nginx php5-fpm php5-mysqlnd php5-cli mysql-server supervisor
RUN sed -e 's/;daemonize = yes/daemonize = no/' -i /etc/php5/fpm/php-fpm.conf
RUN sed -e 's/;listen\\.owner/listen.owner/' -i /etc/php5/fpm/pool.d/www.conf
RUN sed -e 's/;listen\\.group/listen.group/' -i /etc/php5/fpm/pool.d/www.conf
RUN echo "\\ndaemon off;" >> /etc/nginx/nginx.conf
ADD vhost.conf /etc/nginx/sites-available/default
ADD supervisor.conf /etc/supervisor/conf.d/supervisor.conf
ADD init.sh /init.sh
EXPOSE 80 3306
VOLUME \["/srv"\]
WORKDIR /srv
CMD \["/usr/bin/supervisord"\]

我们通过扩展 `debian:wheezy` 这个基础镜像开始，然后通过一系列的`sed`命令来配置Nginx和PHP5-FPM。

RUN sed -e 's/;daemonize = yes/daemonize = no/' -i /etc/php5/fpm/php-fpm.conf
RUN sed -e 's/;listen\\.owner/listen.owner/' -i /etc/php5/fpm/pool.d/www.conf
RUN sed -e 's/;listen\\.group/listen.group/' -i /etc/php5/fpm/pool.d/www.conf
RUN echo "\\ndaemon off;" >> /etc/nginx/nginx.conf

这里我们要做两件事。 首先配置PHP5-FPM和Nginx让他们在前台运行以便supervisord可以追踪到他们。 然后，配置PHP5-FPM以指定的用户运行Web-Server，并处理好文件权限。 接下来需要安装一组配置文件，首先是Nginx的虚拟主机配置文件`vhost.conf`:

server {
    listen 80;
    server\_name \_;
    access_log /var/log/nginx/access.log;
    error_log /var/log/nginx/error.log;
    root /srv/web;
    index app_dev.php;
    location / {
        try\_files $uri $uri/ /app\_dev.php?$query_string;
    }
    location ~ \[^/\]\\.php(/|$) {
        fastcgi_pass unix:/var/run/php5-fpm.sock;
        include fastcgi_params;
    }
}

因为我们不需要域名，所以把`server_name`设成了`_`（有点像perl的`$_`占位符变量）， 并配置根目录（document root）为/svr/web， 我们会把应用程序部署在`/srv`下，剩下的就是标准的Mginx + PHP5-FPM配置. 因为一个容器每次只能运行一个程序， 我们需要supervisord（或者任何别的进程管理器，不过我比较中意supervisord）。幸运的是， 这个进程管理器会产生我们需要的所有进程！下面是一小段supervisord的配置：

\[supervisord\]
nodaemon=true
\[program:nginx\]
command=/usr/sbin/nginx
\[program:php5-fpm\]
command=/usr/sbin/php5-fpm
\[program:mysql\]
command=/usr/bin/mysqld_safe
\[program:init\]
command=/init.sh
autorestart=false
redirect_stderr=true
redirect_stdout=/srv/app/logs/init.log

这里我们需要做的是定义所有的服务， 加上一个特殊的`program:init`进程，它不是一个实际的服务，而是一个独创的运行启动脚本的方式。 这个启动脚本的问题在于，它通常需要先启动某些服务。比如，你可能要初始化一些数据库表，但前提是你得先把MySQL跑起来，一个可能的解决办法是，在启动脚本中启动MySQL，然后初始化表，然后为了防止影响到supervisord的进程管理，需要停掉MySQL，最后再启动supervisord。 这样的脚本看起来类似下面这样：

/etc/init.d/mysql start
app/console doctrine:schema:update --force
/etc/init.d/mysql stop
exec /usr/bin/supervisord

看起来丑爆了有木有，咱换种方式，让supervisor来运行它并且永不重启。 实际的`init.sh`脚本如下：

#!/bin/bash
RET=1
while \[\[ RET -ne 0 \]\]; do
    sleep 1;
    mysql -e 'exit' > /dev/null 2>&1; RET=$?
done
DB\_NAME=${DB\_NAME:-symfony}
mysqladmin -u root create $DB_NAME
if \[ -n "$INIT" \]; then
    /srv/$INIT
fi

脚本先等待MySQL启动，然后根据环境变量`DB_NAME`创建DB，默认为`symfony`， 然后在INIT环境变量中查找要运行的脚本，并尝试运行它。本文的结尾有说明如何使用这些环境变量。

构建并运行镜像
-------

万事俱备只欠东风。我们还要构建Symfony Docker镜像， 使用`docker build`命令:

$ cd docker-symfony
$ docker build -t symfony .

现在，可以使用它来运行你的Symfony工程了:

$ cd SomeProject
$ docker run -i -t -P -v $PWD:/srv symfony

我们来看看这一连串的选项分别是干嘛的:

*   `-i` 启动_交互_（interactive）模式， 也就是说，`STDIO`（标准输入输出）连接到了你当前的终端上。当你要接收日志或者给进程发送信号时，它很有用。
*   `-t` 为容器创建一个虚拟`TTY`， 它跟`-i`是好基友，通常一起使用。
*   `-P` 告诉Docker守护进程发布所有指定的端口， 本例中为80端口。
*   `-v $PWD:/srv` 把当前目录挂载到容器的/srv目录。挂载一个目录使得目录内容对目标挂载点可用。

现在你还记得之前提到的`DB_NAME`和`INIT`环境变量了吧，干嘛用的呢：用于自定义你的环境。 基本上你可以通过 `docker run`的`-e`选项在容器中设置环境变量，启动脚本会拿到环境变量，因此，如果你的DB名为`some_project_dev`， 你就可以这么运行容器：

$ docker run -i -t -P -v $PWD:/srv -e DB\_NAME=some\_project_dev symfony

`INIT` 环境变量就更强大了，它允许你启动时运行指定的脚本。比如， 你有一个`bin/setup`脚本运行`composer install`命令并且设置数据库schema:

#!/bin/bash
composer install
app/console doctrine:schema:update --force

用`-e`来运行它：

$ docker run -i -t -P \
    -v $PWD:/srv \
    -e DB\_NAME=some\_project_dev \
    -e INIT=bin/setup

注意，`-e`选项可以在`docer run`中多次使用，看起来相当酷。另外，你的启动脚本需要可执行权限（`chmod +x`）。 现在我们通过`curl`发送请求到容器，来检查一下是否所有的东西都像预期一样工作。首先，我们需要取到Docker映射到容器的80端口的公共端口，用`docker port`命令:

$ docker port $(docker ps -aql 1) 80
0.0.0.0:49153

`docker ps -aql 1` 是个好用的命令，可以方便的检索到最后一个容器的id， 在我们的例子中，Docker 把容器的80端口映射到了49153端口。我们 curl 一下看看。

$ curl http://localhost:49153
You are not allowed to access this file. Check app_dev.php for more information.

当我们不从localhost（译者注：容器的localhost）访问dev controller时，得到了Symfony的默认错误消息，这再正常不过了， 因为我们不是从容器内部发送 curl 请求的， 所以，可以安全的从前端控制器`web/app_dev.php`中移除这些行。

// This check prevents access to debug front controllers that are deployed by accident to production servers.
// Feel free to remove this， extend it， or make something more sophisticated.
if (isset($\_SERVER\['HTTP\_CLIENT_IP'\])
    || isset($\_SERVER\['HTTP\_X\_FORWARDED\_FOR'\])
    || !(in\_array(@$\_SERVER\['REMOTE\_ADDR'\]， array('127.0.0.1'， 'fe80::1'， '::1')) || php\_sapi_name() === 'cli-server')
) {
    header('HTTP/1.0 403 Forbidden');
    exit('You are not allowed to access this file. Check '.basename(\_\_FILE\_\_).' for more information.');
}

这些行阻止了任何从localhost以外的地方访问dev controller。 现在再curl的时候就可以正常工作了，或者用浏览器访问 http://localhost:49153/：

[![result.png](http://www.php100.com/uploadfile/2015/0105/20150105102412393.png "result.png")](http://www.php100.com/uploadfile/2015/0105/20150105102412393.png)

很容易吧！ 现在我们可以快速的启动、更新环境了，但还是有很多地方需要改进。 （原文链接：[A PHP development environment with Docker](http://geoffrey.io/a-php-development-environment-with-docker.html)）